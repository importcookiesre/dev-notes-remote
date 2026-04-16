#### Method 1:
```
import "dotenv/config";
import mongoose from "mongoose";
export const connectDB = async () => {
	mongoose
		.connect("mongodb://localhost:27017", { dbName: "Ecommerce" })
		.then((db) => {
	console.log("Database connected succesfully : ", db.connection.host);

})
		.catch((err) => {
		console.error("Database connection Failed");
		process.exit(1);
	});

};
```
#### Method 1.1: 
Connecting db in the index.js file.

```
import 'dotenv/config'
import mongoose from "mongoose"
import {DB_NAME} from "constants.js"


import express from "express"
const app = express()

// connecting db
(async () => {
    try {
      const response = await  mongoose.connect(`${process.env.MONGODB_URI}/${DB_NAME}`)
        app.on("error", (error) => {
            console.log("Express Error : ", error);
            throw error
        })

        app.listen(process.env.PORT, () => {
            console.log("app is listening at port ", process.env.PORT);
        })

    } catch (error) {
        console.log("ERROR : " , error);
        throw error 
    }
 })()
```

#### Method 2: 

Write code in db folder and then import it in the index.js file
```
file : db/index.js

import mongoose from "mongoose";
import "dotenv/config"
import { DB_NAME } from "../constants.js"

// DB connection code
const connectDB = async () => {
    try {
        const connectionInstance = await mongoose.connect(`${process.env.MONGODB_URI}/${DB_NAME}`)
        console.log(`\n MonogoDb connected !! DB HOST : ${connectionInstance.connection.host}`);

    } catch (error) {
        console.log("MONGODB connection error ", error);
        process.exit(1)
    }
}


export default connectDB;
```
Now import the db/index.js file in the main file and execute it.
```
// import 'dotenv/config'
import dotenv from "dotenv"
import mongoose from "mongoose"
import { DB_NAME } from './constants.js';
import connectDB from "./db/index.js"

dotenv.config({
    path: "./env"
})


connectDB();
```