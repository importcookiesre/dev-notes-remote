```
import mongoose from "mongoose";

const orderItemSchema = mongoose.Schema({
    producId: {
        type: mongoose.Schema.Types.ObjectId,
        ref: "Product"
    },
    quantity: {
        type: Number,
        default: 0,
        required: true
    }
})


const orderSchema = new mongoose.Schema({
    orderPrice: {
        type: Number,
        default: 0,
        required: true
    },
    customer: {
        type: mongoose.Schema.Types.ObjectId,
        ref: "User"
    },
    orderItems: {
        type: [
            orderItemSchema
        ]
    },
    status: {
        type: String,
        enum: ["PENDING", "CANCELLED", "DELIVERED"],
        default: "PENDING"
    }


}, { timestamps: true })

export const Order = mongoose.model("Order", orderSchema)
```