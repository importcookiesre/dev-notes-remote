


Folder structure : 

```go
├── copilot-instructions.md
├── ISSUE_TEMPLATE
│   ├── bug-report.yml
│   ├── enhancement.yml
│   └── new-feature.yml
├── PULL_REQUEST_TEMPLATE.md
└── workflows
    ├── ci.yaml
    ├── deploy-backend.yaml
    └── deploy-frontend.yaml
```


To create a pull_request_template, simply create a template at `.github/pull_request_template.md`

```md
## Description
<!-- Link your ticket using #{ISSUE_NUMBER}. And add a clear and concise description of what this PR does. -->



## Screenshots (if applicable)
<!-- Add screenshots to help explain your changes. -->

## Checklist
- [ ] I have performed a self-review of my code  
- [ ] I have commented my code, particularly in hard-to-understand areas  
- [ ] I have added tests that prove my fix is effective or that my feature works  
- [ ] New and existing unit tests pass locally with my changes
```

