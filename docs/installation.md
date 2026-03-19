# Installation Guide

This guide covers all supported methods for installing OpenFiltR.

## Quick Install (Curl One-Liner)

The fastest way to get started:

```bash
curl -sSL https://get.openfiltr.dev | sh
```

This script will:
1. Download the latest binary for your platform
2. Install it to `/usr/local/bin`
3. Create a default configuration

### Verification

```bash
openfiltr --version
```

## Docker Installation

### Using Docker Run

```bash
docker run -d \
  --name openfiltr \
  -p 53:53/udp \
  -p 53:53/tcp \
  -p 853:853/tcp \
  -v openfiltr-data:/data \
  openfiltr/openfiltr:latest
```

### Using Docker Compose

Create a `docker-compose.yml` file:

```yaml
version: '3.8'

services:
  openfiltr:
    image: openfiltr/openfiltr:latest
    container_name: openfiltr
    ports:
      - "53:53/udp"
      - "53:53/tcp"
      - "853:853/tcp"
    volumes:
      - openfiltr-data:/data
    restart: unless-stopped

volumes:
  openfiltr-data:
```

Start the service:

```bash
docker-compose up -d
```

## Manual Binary Installation

### Download Binary

Visit the [releases page](https://github.com/openfiltr/openfiltr/releases) and download the appropriate binary for your system:

- Linux (amd64, arm64, armv7)
- macOS (amd64, arm64)
- Windows (amd64)

### Linux/macOS

```bash
# Download
curl -L -o openfiltr https://github.com/openfiltr/openfiltr/releases/latest/download/openfiltr-linux-amd64

# Make executable
chmod +x openfiltr

# Install to PATH
sudo mv openfiltr /usr/local/bin/
```

### Windows

1. Download the `.exe` file
2. Add to PATH or run from any directory

## Raspberry Pi / ARM64

OpenFiltR supports ARM-based devices including Raspberry Pi.

### Raspberry Pi OS

```bash
# ARM64 (Raspberry Pi 3/4/5 64-bit)
curl -L -o openfiltr https://github.com/openfiltr/openfiltr/releases/latest/download/openfiltr-linux-arm64
chmod +x openfiltr
sudo mv openfiltr /usr/local/bin/
```

### ARM32 (Raspberry Pi 1-2)

```bash
curl -L -o openfiltr https://github.com/openfiltr/openfiltr/releases/latest/download/openfiltr-linux-armv7
chmod +x openfiltr
sudo mv openfiltr /usr/local/bin/
```

## DNS Configuration

After installation, configure your router or system to use OpenFiltR as your DNS server.

### Common Router Settings

1. Log into your router admin panel
2. Find "DHCP" or "DNS" settings
3. Set DNS server to your OpenFiltR machine IP

### Linux

```bash
# /etc/resolv.conf
nameserver <your-openfiltr-ip>
```

### macOS

System Preferences → Network → Wi-Fi/Ethernet → Advanced → DNS

## Post-Install Verification

```bash
# Check service status
openfiltr status

# Test DNS query
dig example.com @127.0.0.1
```

## Troubleshooting

### Port 53 Already in Use

If you get an error about port 53:

```bash
# Check what's using port 53
sudo lsof -i :53

# Common culprits: systemd-resolved, bind, dnsmasq
# Stop conflicting services
sudo systemctl stop systemd-resolved
sudo systemctl disable systemd-resolved
```

### Firewall Rules

Allow DNS traffic:

```bash
# UFW
sudo ufw allow 53/tcp
sudo ufw allow 53/udp

# firewalld
sudo firewall-cmd --add-port=53/tcp
sudo firewall-cmd --add-port=53/udp
sudo firewall-cmd --reload
```

### Permission Denied

If you encounter permission issues:

```bash
sudo setcap cap_net_bind_service=+ep /usr/local/bin/openfiltr
```

## Next Steps

- Read the [Configuration Guide](./configuration.md)
- Check the [API Reference](./api.md)
- Join our [Community](https://discord.gg/openfiltr)
