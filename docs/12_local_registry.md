---
layout: default
title: Registry
nav_order: 12
---


# 🔐 Running a Local Docker Registry Over HTTPS with Podman (Raspberry Pi / Linux)

This guide walks you through setting up a local container registry with **TLS/HTTPS** using **Podman**, and ensures it **automatically restarts after reboot**.

---

## 📦 Step 1: Generate Self-Signed TLS Certificates

```bash
mkdir -p ~/certs
cd ~/certs

# Create a self-signed certificate for localhost (valid for 1 year)
openssl req -newkey rsa:4096 -nodes -sha256 -keyout domain.key \
  -x509 -days 365 -out domain.crt \
  -subj "/CN=localhost"

# Optional: Combine into .pem if needed
cat domain.crt domain.key > domain.pem
````

---

## 🧪 Step 2: Run the Registry Container (with HTTPS)

```bash
cd ~/certs

podman run -d --name local-registry \
  --restart=always \
  -p 5000:5000 \
  -v $(pwd)/domain.crt:/certs/domain.crt:ro \
  -v $(pwd)/domain.key:/certs/domain.key:ro \
  -v registry-data:/var/lib/registry \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  docker.io/library/registry:2
```

* `--restart=always` ensures the container restarts after crash or reboot.
* `-v registry-data:/var/lib/registry` persists registry data.

---

## 🔐 Step 3: Trust the TLS Certificate (Podman Client)

### Linux:

```bash
sudo mkdir -p /etc/containers/certs.d/localhost:5000
sudo cp ~/certs/domain.crt /etc/containers/certs.d/localhost:5000/ca.crt
```

---

## 🔄 Step 4: Ensure Container Starts at Boot

Podman uses **systemd** for managing containers.

### Enable lingering for your user (needed for user services to auto-start):

```bash
loginctl enable-linger $USER
```

### Generate and enable the systemd unit:

```bash
podman generate systemd --name local-registry --files --restart-policy=always
mkdir -p ~/.config/systemd/user
mv container-local-registry.service ~/.config/systemd/user/

systemctl --user daemon-reexec
systemctl --user daemon-reload
systemctl --user enable --now container-local-registry.service
```

---

## 🧪 Step 5: Test It

### Push an image:

```bash
podman pull alpine
podman tag alpine localhost:5000/alpine
podman push localhost:5000/alpine
```

### Pull it back:

```bash
podman rmi localhost:5000/alpine
podman pull localhost:5000/alpine
```

---

## 📝 Notes

* Replace `localhost` with your LAN IP or domain if sharing on network.
* For production, use certs from a trusted CA like Let's Encrypt.
* Check logs with:

  ```bash
  podman logs -f local-registry
  ```

---

## 🧼 Cleanup

To stop and remove the registry:

```bash
podman stop local-registry
podman rm local-registry
podman volume rm registry-data
```

To remove systemd unit:

```bash
systemctl --user disable --now container-local-registry.service
rm ~/.config/systemd/user/container-local-registry.service
```

---

## ✅ Summary

You now have:

* A local Podman registry running over HTTPS
* A self-signed TLS certificate
* Automatic restart after reboot using systemd

Happy containerizing! 🐳🔐

```

Let me know if you'd like a PDF version or one tailored for **Colima/macOS** or **Fedora CoreOS**.
```
