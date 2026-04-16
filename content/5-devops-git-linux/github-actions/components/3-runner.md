
The runner is basically a computer instance. It is the server where the shell commands actually runs.


```go
jobs:
  build:
    runs-on: ubuntu-latest    # <--- The Runner
```

**Alternatives:** `windows-latest`, `macos-latest`, or custom labels like `[self-hosted, linux, high-mem]`

By default, you use GitHub's fleet of VMs.
- **Specs (Standard):** 2 vCPUs, 7 GB RAM, 14 GB SSD space.
- **The "Kitchen Sink" Image:** These VMs come pre-loaded with an insane amount of software.
    - **Backend Tools:** Go, Docker, Docker Compose, AWS CLI, `kubectl`, Postgres client, Python, Node, etc.
    - _Benefit:_ You often don't need to install tools, just configure them.
- **Ephemeral (Stateless):** This is the most critical property.
    - Every job gets a **brand new VM**.
    - **Implication:** If Job A installs a library, Job B (running 5 seconds later) _will not have it_. You must use **Caching** or **Artifacts** to persist data.
    - **Security:** Excellent. You don't worry about "cleaning up" sensitive files; the VM is destroyed immediately after the job.


## self hosted : 


