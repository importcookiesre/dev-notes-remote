
The hierarchy of components : 
**Workflow file (`.yml`)** $\rightarrow$ **Triggers (Events)** $\rightarrow$ **Jobs** $\rightarrow$ **Runners** $\rightarrow$ **Steps** $\rightarrow$ **Actions**


## 1. Workflow : 

- It is the entire automation pipeline defined in a YML file.
	- `.github/workflows/name.yml`
- We might have one workflow named `ci.yml` for testing and linting every time code is pushed, and a separate workflow named `deploy.yml` that only runs when you merge to the `main` branch.
- This file might say:
	- When code is pushed
	- Run tests
	- Build the app


## 2. Event : 

A workflow doesn't run unless something kicks it off. An event is a specific activity in a repository that triggers a workflow run.

- Defined by `on:` keyword
- **Common Events:** `push`, `pull_request`, `schedule` (cron jobs), `workflow_dispatch` (manual button click).
- We can filter these. For example, trigger `on: push` but **only** for `branches: [ "main" ]` or only if changes happen in `backend/**`.


## 3. Jobs

A workflow is made of one or more jobs. By default, jobs run **in parallel**.
- **Key Concept:** Each job runs in its own isolated environment (Runner). They do not share memory or files unless you explicitly use "Artifacts" to pass data between them.
- **Dependencies:** You can force order using `needs: [job_name]`.


## 4. Runners (the server)

The runner is the server that actually executes the job.
- **Types:**
    
    - **GitHub-hosted:** Virtual machines provided by GitHub (Ubuntu, Windows, macOS). They are fresh instances every time (stateless).
        
    - **Self-hosted:** Your own servers (e.g., an EC2 instance or a distinct container) that you connect to GitHub. Useful for heavy caching or accessing private network resources (like a private database).



## 5. Steps : 

Inside a specific Job, steps execute **sequentially** (one after another).

- **Context:** Since steps run on the same Runner, they **share the filesystem**. If Step 1 compiles a binary, Step 2 can run that binary.


## 6. Actions

An "Action" is a custom application for the GitHub Actions platform that performs a complex but frequently repeated task.

- **Syntax:** `uses: name/action@version`
    
- **Why use them?** Instead of writing a complex 20-line Bash script to log into Docker Hub or set up Go, you use a pre-packaged Action.

