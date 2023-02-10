# Install Base agent on Ubuntu/Debian

## 1. Installing the package

> **sudo** permission is required.

<!-- 
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/eKiRRnym7KRW98Y/download/dsk-node-agent-install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-node-agent

sudo DSK_GLOBAL_APIKEY=${DSK_GLOBAL_APIKEY} bash -c '/usr/bin/dsk-node-agent init "'${DSK_GLOBAL_APIKEY}'" && systemctl enable dsk-node-agent --now'
```

## 2. Checking the status of package

```bash
systemctl status dsk-node-agent
```

or use `service` command

```bash
service dsk-node-agent status
```

## 3. Stopping the package

```bash
systemctl stop dsk-node-agent
```

or use `service` command

```bash
service dsk-node-agent stop
```

## 4. Uninstalling the package

```bash
sudo apt remove dsk-node-agent
```
