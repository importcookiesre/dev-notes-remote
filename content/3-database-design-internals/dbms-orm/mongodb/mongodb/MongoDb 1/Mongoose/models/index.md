
- **check for valid mongoDb object id:**
```ts
const isValidObjectId = mongoose.Types.ObjectId.isValid(id);
if (!isValidObjectId) {
	return next(new ApiError("Invalid Object id", 400));
}
```


### Best practices : 


- Have seperate routes and access token secret for users and admins for better security.
	- api/v1/user
	- api/v1/admin
- 