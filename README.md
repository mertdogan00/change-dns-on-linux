
# 🔧 Change DNS on Linux using systemd-resolved

A step-by-step guide to change and persist DNS settings on any Linux distribution using **systemd-resolved**. This method is clean, reliable, and avoids conflicts caused by manually editing `/etc/resolv.conf`.

---

## 📋 Prerequisites

- A Linux system using `systemd` (e.g., Ubuntu 20.04+, Debian 11+, Arch, etc.)
- Root privileges

---

## 🚀 Steps to Change DNS

### 1️⃣ Check if systemd-resolved manages DNS

```bash
ls -l /etc/resolv.conf
```

**Expected Output:**
```
/etc/resolv.conf -> ../run/systemd/resolve/stub-resolv.conf
```

➡️ If you see a symlink like above, you're good to go.

---

### 2️⃣ Edit DNS configuration

Open the resolved configuration file:

```bash
sudo nano /etc/systemd/resolved.conf
```

Update or add the following under the `[Resolve]` section:

```ini
[Resolve]
DNS=1.1.1.1 1.0.0.1
FallbackDNS=8.8.8.8 8.8.4.4
DNSSEC=no
MulticastDNS=yes
```

Save and exit (`Ctrl + O`, `Enter`, then `Ctrl + X`).

---

### 3️⃣ Restart the service

Apply your changes by restarting `systemd-resolved`:

```bash
sudo systemctl restart systemd-resolved
```

---

### 4️⃣ Verify DNS is active

Check current DNS settings:

```bash
resolvectl status
```

**You should see something like:**
```
DNS Servers: 1.1.1.1
             1.0.0.1
Fallback DNS: 8.8.8.8
              8.8.4.4
DNSSEC: no
MulticastDNS: yes
```

---

## 🧪 Test Connectivity

### 🔹 Ping a website

```bash
ping -c 3 google.com
```

### 🔹 Check DNS resolution

```bash
dig github.com
```

### 🔹 Verify HTTPS access

```bash
curl -I https://raw.githubusercontent.com
```

---

## 📁 Optional: View /etc/resolv.conf

```bash
cat /etc/resolv.conf
```

You’ll likely see:

```
nameserver 127.0.0.53
```

ℹ️ This is normal with systemd-resolved. Don’t edit this file manually.

---

## ✅ Success

✔ DNS changed to **Cloudflare** and **Google**  
✔ Configuration is persistent and reliable  
✔ Connectivity and DNS lookups verified  

---

## 💻 Tested On

- Ubuntu 20.04, 22.04
- Debian 11, 12
- Arch Linux (with systemd)
- Any distro using `systemd-resolved`

---

## Contributing 🤝

Have suggestions or improvements? Feel free to open an issue or submit a pull request!

---

## License 📜

This project is licensed under the MIT License.  
See the [LICENSE](LICENSE) file for more information.

