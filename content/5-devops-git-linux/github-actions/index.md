
1. Quotes are optional but recommanded
```yml
branches:
  - "prod"
  - "main"
  - "master"
```

2. Allows both inline list and block list :
```yml
on:
  pull_request:
    branches: ["main", "prod", "master"]
    
on:
  pull_request:
    branches:
      - "main"
      - "prod"
      - "master"
```

