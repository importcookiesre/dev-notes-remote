---
title: Github Actions
description: Github Actions Notes
draft: false
tags:
  - backend
  - devops
  - automation
  - github
  - CI/CD
modified: 2026-04-16
created: 2026-04-16
---


- Go tests : [[1-go-test]]
- React vite test : [[react-vite-build]]



# problems : 


## 1. How to pass data forward ? 

### 1) Step → later steps in the same job

Use **`GITHUB_ENV`** when you want to set an environment variable that later steps in the same job can read. 
Use **`GITHUB_OUTPUT`** when a step should expose a named output for other steps to read through `steps.<step_id>.outputs.<name>`.

```yml
- id: prep
  run: echo "STORE_PATH=$(pnpm store path --silent)" >> "$GITHUB_OUTPUT"

- run: echo "${{ steps.prep.outputs.STORE_PATH }}"
```

### 2) Job → another job in the same workflow

Use **job outputs**. 
First map a step output to `jobs.<job_id>.outputs`, then read it in the next job with `needs.<job_id>.outputs.<name>`. 
GitHub’s docs show this exact `jobs.<job_id>.outputs` + `needs.<job_id>.outputs.<output_name>` pattern.


### 3) Share files or build results between jobs

Use **artifacts** when the thing you want to pass is a file, build output, test report, or log. GitHub says artifacts are for sharing data between jobs in a workflow and for storing data after the workflow completes. The pair is `actions/upload-artifact` and `actions/download-artifact`.

### 4) Workflow → reusable workflow / another workflow

Use **`on.workflow_call` inputs and outputs** for reusable workflows. GitHub says `on.workflow_call` defines inputs and outputs for a reusable workflow, and those outputs are available to downstream jobs in the caller workflow. Reusable workflows are different from composite actions: reusable workflows are whole workflows, while composite actions are just steps inside a job.

### 5) Configuration values that are available during the run

Use **`env`**, **`vars`**, and **`secrets`** for values that are not produced at runtime but should be available to the workflow. GitHub’s contexts reference lists `env`, `vars`, `secrets`, `needs`, `steps`, and `inputs` as separate contexts, and the variables docs say `env` scope can be workflow/job/step-level.

### 6) Not for passing data, but for reuse

Use **caching** when the data changes rarely, such as package manager dependencies. GitHub explicitly says caching is for files that do not change often between jobs or workflow runs, while artifacts are for things like built binaries or logs. So cache is performance/reuse; artifacts are data transfer.

### Clean rule of thumb

Use the **smallest scope that works**:

- same job value → `GITHUB_ENV` or `GITHUB_OUTPUT`
- next job value → job outputs + `needs`
- file between jobs → artifacts
- shared workflow contract → reusable workflow inputs/outputs
- static config → `env` / `vars` / `secrets`
- dependency reuse → cache



### What is `GITHUB_OUTPUT` ?

`$GITHUB_OUTPUT` is a special file GitHub Actions gives to a step so that the step can publish **outputs** for later steps to read. Writing `name=value` into that file sets a step output. GitHub documents this as the supported way to set output parameters in workflows.

In your workflow, this line:

echo "STORE_PATH=$(pnpm store path --silent)" >> $GITHUB_OUTPUT

creates a step output named `STORE_PATH` for the step with `id: pnpm-cache`. Then this later reference:

${{ steps.pnpm-cache.outputs.STORE_PATH }}

reads that value and passes it into the cache step as the directory to cache.

So the flow is: one step computes a value, writes it to `$GITHUB_OUTPUT`, and a later step uses `steps.<step_id>.outputs.<output_name>` to access it. That is different from `$GITHUB_ENV`, which is for environment variables inside the job.

A tiny mental model helps: `$GITHUB_OUTPUT` is for **passing data forward from one step to another**, not for printing logs and not for global storage.




## 2. What is the need of caching ? How caching works ?


**How caching works in your workflow**

`actions/cache@v4` does two things for you:
1. **Restores** cached files at the start of the job if the key matches.
2. **Saves** the files back at the end of the job if needed.

So in your workflow:
- the **pnpm store cache** is restored into the store path
- the **node_modules cache** is restored into `packages/web/node_modules`
- then your later steps, especially `pnpm install`, `pnpm lint`, and `pnpm build`, use those restored files automatically

- caches are used across later workflow runs. 


Example :
Here if the pnpm store is already cached, `pnpm install` will reuse those packages instead of downloading them again.
```yml
name: Install dependencies
working-directory: packages/web
run: pnpm install --frozen-lockfile
```



## 3. Failures in the CI :

1. A CI Failure is usually caused by a step failing first, and then that failure can make the job fail, which in turn makes the workflow run fail.

> [!question] What is a failure ? 
> A step or action is considered failed when it returns a **non-zero exit code.**


## 4. Isolation : 

1. Each job runs in an isolated seperate runner VM or container.
2. Each steps in a job runs in on that same runner but on a different process so the next step does not have access to the variables used.

