
# scheduling

> **Script scheduling** in Unix-like systems refers to the process of configuring a script to execute **automatically at a specific time, interval, or event**, using tools like `cron`, `at`, or `systemd timers`.


| Tool             | Purpose                           | Best for                       |
| ---------------- | --------------------------------- | ------------------------------ |
| `cron`           | Repeated execution at fixed times | Recurring jobs (daily, hourly) |
| `at`             | One-time delayed execution        | Run once in the future         |
| `systemd timers` | Modern, flexible scheduling       | Service-linked & advanced jobs |
| `sleep & loop`   | Manual delay + loop in script     | Simple repeated tasks          |



## `at` : 

- For scheduling only one time

**create a schedule**
```shell
at 02:45 PM  # at time
echo "hello world" < "file.txt"  # command
Ctrl + D  # exit 
```

**check all schedule**
```shell
atq 
```

**delete a schedule**
```shell
atrm 2  # atrm <jobid>
```

```
at now + 5 minutes
at 4pm
at 11pm tomorrow
at 10:30am < task.sh

```

|Format|Example|Meaning|
|---|---|---|
|`now + N minutes`|`now + 10 minutes`|10 minutes from now|
|`HH:MM`|`14:30`|Today at 2:30 PM|
|`4pm tomorrow`||4 PM next day|
|`midnight`||12:00 AM today|
|`noon`||12:00 PM today|
|`next week`||Same time, 7 days later|



## cron

- use to repeately execute / periodically scripts
- 