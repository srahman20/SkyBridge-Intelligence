# Server Setup

This document outlines the steps to provision and configure the virtual machine (VM) that hosts the SkyBridge Intelligence backend services, including AI model interfacing and API endpoints.

---

## Virtual Machine Creation (Ubuntu Server)

| **Setting**           | **Value**                       |
|-----------------------|---------------------------------|
| VM Name               | `skybridge-vm`                  |
| Image                 | Ubuntu 22.04 LTS                |
| Size                  | Standard B2s (2 vCPU, 4 GB RAM) |
| Region                | Same as resource group          |
| Authentication Type  | SSH Public Key                  |
| Inbound Ports         | SSH (22), HTTP (80)             |
| OS Disk Size          | 64 GB                           |
| Virtual Network       | `skybridge-vnet`                |
| Subnet                | `skybridge-subnet`              |
| Public IP             | `skybridge-ip` (Dynamic/Static) |
| NIC                   | `skybridge-nic`                 |
| NSG                   | `skybridge-nsg`                 |

---

## SSH Access

```bash
ssh -i ~/.ssh/your_private_key.pem azureuser@<public-ip-address>
```

Replace <public-ip-address> with the IP assigned to skybridge-ip.

## Package Installation
Install necessary packages on the server:

```
sudo apt update && sudo apt upgrade -y
sudo apt install python3-pip python3-venv nginx git -y
```

# Python Environment Setup

```
python3 -m venv skybridge-env
source skybridge-env/bin/activate
pip install flask openai azure-identity
```

## Clone & Run App

```
git clone https://github.com/yourusername/skybridge-intelligence.git
cd skybridge-intelligence/backend
python3 app.py
```
Optional: NGINX Reverse Proxy (Port 80)
Configure NGINX to route HTTP traffic to your Flask app (running on port 5000):

```
sudo nano /etc/nginx/sites-available/skybridge
```
Example config:

```
server {
    listen 80;
    server_name your_domain_or_ip;

    location / {
        proxy_pass http://127.0.0.1:5000;
        include proxy_params;
        proxy_redirect off;
    }
}
```
## Enable and reload:

```

sudo ln -s /etc/nginx/sites-available/skybridge /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

## Notes
Use firewall rules or NSG to secure access.

SSL with Let's Encrypt is recommended for production.

Ensure Python app binds to 0.0.0.0 if accessed externally.
