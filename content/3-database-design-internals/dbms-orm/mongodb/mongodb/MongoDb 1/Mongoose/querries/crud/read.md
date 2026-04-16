
#### select()

**Make db call and exclude some field**
```ts
UserModel.findOne({ username }).select('-password -refreshToken');
```

**Make db call and fetch some specific fields**
```ts
UserModel.findOne({ username }).select('password refreshToken');
```

