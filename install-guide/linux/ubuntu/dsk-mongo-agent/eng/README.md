# Installing Mongo Agent on Ubuntu/Debian

## How to use the package

### 1. Installing the package

> **sudo** permission is required.

<!-- 
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/D6SFsRsQfYJHedd/download/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-mongo-agent

sudo DSK_GLOBAL_APIKEY=${DSK_GLOBAL_APIKEY} bash -c '/usr/bin/dsk-mongo-agent init "'${DSK_GLOBAL_APIKEY}'" && systemctl enable dsk-mongo-agent --now'
```

### 2. Checking the status of package

```bash
systemctl status dsk-mongo-agent
```

or use `service` command

```bash
service dsk-mongo-agent status
```

### 3. Stopping the package

```bash
systemctl stop dsk-mongo-agent
```

or use `service` command

```bash
service dsk-mongo-agent stop
```

### 4. Uninstalling the package

```bash
sudo apt remove dsk-mongo-agent
```

## How to change the agent settings

You can change the agent settings to monitor MongoDB.

### Arguments

To change the agent settings, you can add or remove the arguments.

Please refer to the [relevant documents](../../../../settings/dsk-mongo-agent/settings.md).
