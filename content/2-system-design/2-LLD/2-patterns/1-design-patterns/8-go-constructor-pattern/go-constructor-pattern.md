
- A constructor in Go is **just a function**
- It **returns a fully initialized struct**
- Naming convention: `New<TypeName>`
- we can add validation 


## 1. **simplest example :**
```go
type User struct {
	ID    string
	Email string
	Age   int
}

func NewUser(id, email string, age int) *User {
	return &User{
		ID:    id,
		Email: email,
		Age:   age,
	}
}
```

usage :
```go
user := NewUser("u1", "test@example.com", 25)
```

## 2. with validation : 

Why this is important:
- Invalid state is **impossible**
- Errors are handled at creation time
- Objects are always safe to use afterward

```go
var ErrInvalidUser = errors.New("invalid user")

func NewUser(id, email string, age int) (*User, error) {
	if id == "" {
		return nil, ErrInvalidUser
	}

	if age < 18 {
		return nil, ErrInvalidUser
	}

	return &User{
		ID:    id,
		Email: email,
		Age:   age,
	}, nil
}
```

usage : 
```go
user, err := NewUser("u1", "test@example.com", 25)
if err != nil {
	log.Fatal(err)
}
```