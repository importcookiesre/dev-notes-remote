```
import mongoose from "mongoose"

const patientSchema = new mongoose.Schema({
    name: {
        type: String,
        requried: true
    },
    diagnosedWith: {
        type: String,
        requried: true
    },
    address: {
        type: String,
        requried: true
    },
    age: {
        type: Number,
        requried: true
    },
    bloodGroup: {
        type: String,
        enum: ["A+", "A-", "B+", "B-", "O+", "O-", "AB+", "AB-"],
        requried: true,
    },
    gender: {
        type: String,
        enum: ["M", "F", "O"],
        required: true
    },
    admittedIn: {
        type: mongoose.Schema.Types.ObjectId,
        ref: "Hospital"
    }
}, { timestamps: true })

export const Patient = mongoose.model("Patient", patientSchema)
```