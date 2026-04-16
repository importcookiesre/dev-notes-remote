
- An event triggers a workflow.
- We can filter the events to run on changes in specific branches or directories.


```yaml
on:
  push:                # Trigger 1: Code Push
  pull_request:        # Trigger 2: PR activity
  schedule:            # Trigger 3: Time-based (Cron)
  workflow_dispatch:   # Trigger 4: Manual button click
```

If we use `on: push` for feature branches _and_ `on: pull_request`, we might trigger **double runs** (one for the branch push, one for the PR update).
- Usually `push` event is used only in main or protected branches and `pull_request` on every commit.


## 1. Types of events : 
### 1. push : 

- **Behavior:** Runs every time a commit is pushed to the repo.
- **Context:** It runs against the specific commit (`sha`) on that branch.
- **Backend Use Case:** "Continuous Integration." You want to know immediately if the latest commit broke the `main` branch.





### 2. pull_request : 

- **Behavior:** Runs when a PR is opened or updated.
- **Context:** By default, GitHub Actions checks out a **merge commit** (a simulation of what would happen if you merged the PR into the target branch right now). This is critical because it tells you if your changes act correctly _after_ being combined with the latest code.




### 3. manual & external events : 

#### A. `workflow_dispatch`

This enables a "Run workflow" button in the GitHub UI. It allows you to define inputs (variables) that you can type in before running.
- **Backend Use Case:** Manual Rollbacks or Database Migrations.

```yml
on:
  workflow_dispatch:
    inputs:
      logLevel:
        description: 'Log Level'
        required: true
        default: 'warning'
      environment:
        description: 'Target Environment'
        type: choice
        options:
        - staging
        - production
```

#### B. `repository_dispatch` (The API Trigger)

This allows your workflow to be triggered by an external HTTP request (curl/Postman) sending a specific event type.

- **Use Case:** You have a CMS (Content Management System). When a content editor hits "Publish" on the CMS, it sends a webhook to GitHub to trigger a static site rebuild or a cache invalidation workflow.


### 4. Schedule : Cron 

we can run workflow using cron 

```yml
on:
  schedule:
    - cron: '30 2 * * *' # Runs at 2:30 AM UTC every day
```
- **Backend Use Case:**
    - Nightly security scans (`trivy` or `snyk`).
    - Cleaning up old Docker images.
    - Generating weekly reports.
- _Note:_ GitHub cron is not perfectly precise. It can be delayed by 10-20 minutes during high load times. Do not use it for time-critical executions.

## 2. Filters : 

- We dont want to run the workflow on eveything


### 1. path filtering : `paths` / `paths_ignore`

- If we make changes on `README.md`, `Docs` or the `frontend/` folder, we dont want to wait 10 mins to run the CI, for this we use path filtering : 

```yml
on:
  push:
    paths:
      - 'backend/**'      # Only run if files in backend/ change
      - 'go.mod'          # Or if dependencies change
      - '!backend/docs/**' # BUT ignore the docs folder inside backend
```


### 2. Branch filtering : `branches` / `branches-ignore`


```yml
on:
  push:
    branches:
      - 'main'        # Deploy to Production
      - 'releases/**' # Deploy to Staging (matches releases/v1, releases/beta)
```


### 3. Activity types : 

- Sometimes `on: pull_request` is too broad. We can filter a PR based on its state : `opened` , `closed`, `labeled` ,`assigned`
- For example : We have an expensive CI workflow that requires a full size infrastructure, its costs on every execution. No obviously we dont want to run it on every type, docs changes, right. So we can filter this based on activity type like `ready_for_review`


```yml
on:
  pull_request:
    types: [opened, ready_for_review]
```


**Common Types:**

- `synchronize`: Triggered when new commits are pushed to an existing PR.
- `closed`: Useful for cleanup jobs (e.g., destroying a temporary Docker staging environment when a PR is merged or closed).

