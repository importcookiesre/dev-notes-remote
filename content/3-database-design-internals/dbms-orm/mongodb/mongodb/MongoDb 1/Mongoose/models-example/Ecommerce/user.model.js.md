```
import mongoose from "mongoose"

const userSchema = new mongoose.Schema({
    username: {
        type: String,
        requried: true,
        lowercase: true,
        unique: true
    },
    email: {
        type: String,
        requried: true,
        lowercase: true,
        unique: true
    },
    password: {
        type: String,
        required: true,
    },
    address: {
        type: String,
        required: true,
    }
}, {timestamps : true})


export const User = mongoose.model("User" , userSchema)
```