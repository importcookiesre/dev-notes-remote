
## SCM Polling (“Poll SCM”)

- **What it does**: Jenkins periodically checks your Source Control (Git, SVN, Mercurial, etc.) for changes.
- **Configuration**:
    
    - In Freestyle: check **“Poll SCM”** and supply a cron-style schedule (e.g. `H/15 * * * *` to poll every 15 minutes).
    - In Declarative Pipeline:

```
triggers { 
  pollSCM('H/15 * * * *') 
}
```

- **Notes**:
    - Polling only _queries_ the SCM; it does **not** fetch or execute a build unless changes are detected.
    - Use the `H/…` “hash” token to spread out load when you have many jobs.

