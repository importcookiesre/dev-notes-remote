```
import mongoose from "mongoose"

const doctorSchema = new mongoose.Schema({
    name: {
        type: String,
        requried: true
    },
    salary: {
        type: String,
        requried: true
    },
    qualification: {
        type: [String],
        requried: true
    },
    experienceInYear : {
        type: Number,
        requried: true,
        default: 0
    },
    worksIn: {
        type: mongoose.Schema.Types.ObjectId,
        ref: "Hospital"
    }

}, { timestamps: true })

export const Doctor = mongoose.model("Doctor", doctorSchema)
```