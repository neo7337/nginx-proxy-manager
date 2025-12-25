# 🚀 Nginx Proxy Manager (NPM) Setup Guide

This project provides a robust, production-ready boilerplate for managing multiple domains on a single IP address using **Nginx Proxy Manager** in a Dockerized environment.

## 📦 What's Inside?

- Nginx Proxy Manager: A sleek Web UI for managing Nginx proxy hosts.

- Dual App Demo: Two dummy application containers (app-red and app-blue) to demonstrate domain-based routing.

- Custom Network: A dedicated Docker bridge network (proxy-tier) for secure internal communication.

## 🛠️ Getting Started

1. Prerequisites

- Docker and Docker Compose installed.
- Basic knowledge of terminal/CLI.

1. Project Structure

```txt
.
├── docker-compose.yml    # Main orchestration file
├── data/                 # NPM database and settings (persistent)
└── letsencrypt/          # SSL certificates (persistent)
```

1. Deployment

Run the stack using Docker Compose:

```bash
docker compose up -d
```

1. Configure Local DNS

Since these domains aren't live on the internet, point them to your local machine by editing your `hosts` file:

- Linux/macOS: `sudo nano /etc/hosts`
- Windows: Notepad (as Admin) -> `C:\Windows\System32\drivers\etc\hosts`

Add the following:

```
127.0.0.1  domain1.local
127.0.0.1  domain2.local
```

## 🖥️ Usage

**Accessing the Dashboard**

1. Open `http://localhost:81` in your browser.

2. Default Credentials:

- Email: <admin@example.com>
- Password: changeme

1. Follow the prompts to update your profile and password immediately.

**Adding your first Proxy Host**

1. Go to **Proxy Hosts > Add Proxy Host**.
2. Domain Names: domain1.local
3. Forward Hostname/IP: app-red (this is the container service name).
4. Forward Port: 80
5. Click Save.

## 🔒 SSL Management

**Local Development (Self-Signed)**

For local testing, you can generate a self-signed certificate:

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout local-key.pem -out local-cert.pem \
  -subj "/CN=domain1.local"
```

Upload these files under the SSL Certificates tab in the NPM dashboard.


**Production (Let's Encrypt)**

NPM automates Let's Encrypt certificates.

- Ensure Ports 80 and 443 are open on your router/firewall.
- In NPM, select Request a new SSL Certificate when adding a Proxy Host.
- NPM will automatically handle renewals every 60-90 days.

## ❓ FAQ

**Q: Can I point hundreds of domains to this one IP?** 

A: Yes. Nginx uses the "Host Header" in the HTTP request to determine which container should receive the traffic.

**Q: My browser says "Not Secure" on my local site!**

A: This is normal for self-signed certificates. You can safely click "Advanced" -> "Proceed anyway," or manually trust the certificate in your OS keychain.

**Q: How do I see my admin password if I'm locked out?**

A: Passwords are encrypted. You can reset to default by running: docker exec -it <container_name> npm-admin-reset

**Q: Why isn't Let's Encrypt working on my local machine?**

A: Let's Encrypt requires a publicly accessible domain and IP to verify ownership. For local dev, use Self-Signed certs or a DNS Challenge with a real domain.

## 📄 License

This project is open-source and available under the MIT License.
