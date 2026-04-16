
Prisma is an open-source **orm**. 
- **Prisma Client:** 
	- The **Prisma Client** is an auto-generated, type-safe, and modern **ORM (Object-Relational Mapping)** client that allows you to interact with your database in **TypeScript or JavaScript** using a fluent API.
	- It can be used to : 
		- **Query** your database
		- **Create, update, delete** records
		- **Join related tables** via relations
		- **Ensure type safety** and auto-complete in your editor




### Setup

initialize a ts project and then add the prisma cli as dev dependency:
```
npm install prisma --save-dev
```

initialize prisma : 
```
npx prisma init --datasource-provider postgresql 
```
- creates a prisma dir containing schema.prisma file .
- creates a .env file containing db connection string.

install **prisma** **client** : 
```
npm install @prisma/client
```
To update the prisma client use : 
```
npx prisma generate
```
**prisma migrate**

- To map our data model to the database schema, we need to use the `prisma migrate` CLI commands:

```
npx prisma migrate dev --name init
```

1. It creates a new SQL migration file for this migration
2. It runs the SQL migration file against the database.
3. It runs `prisma generate` under the hood.


### how to use prisma ?

```ts
import { PrismaClient } from './generated/prisma'

const prisma = new PrismaClient()

async function main() {
	const user = await prisma.user.create({
				data : {
					name  : "suraj"
				}
				})  
	console.log(user)				
}

main()
  .then(async () => {
    await prisma.$disconnect()
  })
  .catch(async (e) => {
    console.error(e)
    await prisma.$disconnect()
    process.exit(1)
  })
```


## 3. prisma cli : 


#### 1. initialization

- `prisma init` : 
	- Initializes a new prisma project in the current directory. Creates a `.env` file with db config.



#### 2. migration : 

- `prisma migrate dev` : 
	- Performs database migration to match the latest prisma schema. Automaticlly creates new tables in the db.

-  `prisma migrate generate`
	- Generates migration scripts for database upgrades/downgrades.

* `prisma migrate deploy`: 
	* Deploys changes from the local development environment to the production environment (if using a CI/CD pipeline).


#### 3. schema and model management.



#### 4. client :

-  `prisma generate` :
	- Regenerate the prisma client based on the current schema. rebuilds the client for database interactions.


#### 5. db connection : 

- `prisma db connect` : 
	- Connects to a database instance using the provided connection string or environment variable.

- `prisma db disconnect`  : 
	- disconnects from a database instance.

