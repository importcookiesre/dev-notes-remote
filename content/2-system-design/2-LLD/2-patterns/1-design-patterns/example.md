

write db connection of postgres and mongo db in seperate file and use a factory function to initialize the global db client based on a env variable. follow startegy pattern, factory, singleton, etc



## strategy : 

mongo

```ts
import mongoose from "mongoose";
import logger from "../logger.js";
import dotenv from "dotenv";

dotenv.config();

const uri = process.env.MONGO_URI;
if (!uri) {
	throw new Error("Missing MONGO_URI environment variable.");
}

export const connectMongo = async () => {
	try {
		await mongoose.connect(uri);
		logger.info("[MONGO] : Connected to MongoDB database using Mongoose.");
		return mongoose.connection;
	} catch (error) {
		logger.error("[MONGO] : Failed to connect to MongoDB using Mongoose:", error);
		process.exit(1);
	}
};

```


postgres : 

```ts
import { PrismaClient } from "@/generated/prisma/client";
import logger from "../logger.js";

const client = new PrismaClient();

export const connectPostgres = async () => {
  try {
    await client.$connect();
    logger.info("[PRISMA] : Connected to PostgreSQL database.");
    return client;
  } catch (error) {
    logger.error("[PRISMA] : Failed to connect to PostgreSQL:", error);
    process.exit(1); // Exit process on a critical error
  }
};

```



## factory and skeleton :


```ts
import { PrismaClient } from "@/generated/prisma/client";
import { Connection as MongooseConnection } from "mongoose"; //
import { connectPostgres } from "./postgres-strategy.js";
import { connectMongo } from "./mongo-strategy.js";

// Define a union type that represents all possible database clients
type DBClient = PrismaClient | MongooseConnection;

// Global singleton instance
declare global {
  var dbGlobal: DBClient | undefined;
}

const dbClientFactory = async (dbType: string): Promise<DBClient> => {
  if (global.dbGlobal) {
    return global.dbGlobal;
  }

  let client: DBClient;
  switch (dbType) {
    case "POSTGRES":
      // connectPostgres returns a PrismaClient instance
      client = await connectPostgres();
      break;
    case "MONGO":
      // connectMongo returns a MongooseConnection instance
      client = await connectMongo();
      break;
    default:
      throw new Error(`Unsupported DB type: ${dbType}`);
  }

  // Store the client in the global scope for reuse during hot reloading
  if (process.env.NODE_ENV !== "production") {
    global.dbGlobal = client;
  }

  return client;
};

export default dbClientFactory;

```


## how to use it : 


```ts
import dbClientFactory from "./database/db-factory.js";
import { PrismaClient } from "@/generated/prisma/client";
import { Connection as MongooseConnection } from "mongoose";

const startServer = async () => {
  const dbType = process.env.DATABASE_TYPE;
  if (!dbType) {
    throw new Error("DATABASE_TYPE environment variable is not set.");
  }

  const dbClient = await dbClientFactory(dbType);

  if (dbClient instanceof PrismaClient) {
    // You now have IntelliSense and type safety for Prisma methods
    const users = await dbClient.user.findMany();
    console.log(users);
  } else if (dbClient instanceof MongooseConnection) {
    // You have IntelliSense and type safety for Mongoose methods
    const users = await dbClient.model("User").find({});
    console.log(users);
  }
};
```