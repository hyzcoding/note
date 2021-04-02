# pm2

## 安装

```bash
> npm install pm2@latest -g
```

## 启动

```bash
> pm2 start app.js
```

## 启动配置

```bash
# Specify an app name
--name <app_name>

# Watch and Restart app when files change
--watch

# Set memory threshold for app reload
--max-memory-restart <200MB>

# Specify log file
--log <log_path>

# Pass extra arguments to the script
-- arg1 arg2 arg3

# Delay between automatic restarts
--restart-delay <delay in ms>

# Prefix logs with time
--time

# Do not auto restart app
--no-autorestart

# Specify cron for forced restart
--cron <cron_pattern>

# Attach to application log
--no-daemon
```

## 查看

```bash
 > pm2 [list|ls|status]
```

[官网]: https://pm2.keymetrics.io/docs/usage/quick-start/

