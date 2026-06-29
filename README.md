# WireGuard Unified Management Portal Console

This system is a production-grade management portal with full bilingual support, allowing you to monitor and control a live WireGuard (WG) server in real time, batch-provision connecting peers, and dynamically auto-generate client `.conf` configurations.

## ✨ Features
1. **Live Real-Time Synchronization**:
   - Parses the `wg show dump` command on the server every 2 seconds to automatically update the transmission traffic (Rx/Tx), current transfer rate, and latest handshake timestamp for each terminal in real time.
2. **Comprehensive Setup Wizard**:
   - Upon first launch, if the configuration does not exist, the portal automatically walks you through a beautiful, robust installer. Here, you can define the admin username and password, interface name, gateway CIDR, external domain/endpoint, DNS, MTU, persistent keepalive, and cryptographic key pairs.
3. **Flexible Automatic Peer Generation & Existing Key Imports**:
   - Fully supports "New Peer Registration" (with automatic secure cryptographic key generation), "Manual Existing Peer Import" (to bring your own client keys), and "Batch Synchronization Import" (parsing and importing multiple peers from an existing WireGuard configuration file).
   - Unnecessary administrative fields (such as "Department" or "Email") are optional, optimizing management strictly to the client name, OS type, and device model.
4. **Resilient Safe Fallback & Local Sandbox Mode**:
   - In containerized, sandbox, or development environments where the WireGuard kernel driver or `wg` command is not found, the server automatically falls back to a highly realistic, interactive JSON database mode instead of crashing.

---

## 🛠️ Prerequisites & Manual Setup (For Live Server Deployment)

To run this application on a production server and reflect configurations to WireGuard in real-time, please complete the following manual configuration steps.

### 1. Granting `sudo` Permissions for the `wg` Command
To allow the Node.js process user (e.g., `node` or `ubuntu`) to execute the `wg` command non-interactively without prompting for a password, configure `sudoers`.

Run `sudo visudo` in your terminal and append the following configuration at the end:

```bash
# Allow the Node.js system user to run wg commands without a password prompt
<your_system_user> ALL=(ALL) NOPASSWD: /usr/bin/wg, /usr/sbin/wg
```
*Note: The path to `wg` may vary. Verify the correct path with `which wg` and adjust accordingly.*

### 2. Opening Router & Firewall Ports
Ensure that the WireGuard UDP listening port (default: `51820`) and this web UI listening port (`3000`) are allowed through your router port-forwarding and Linux firewalls (`ufw` or `iptables`).

```bash
# Example: Configuration using UFW
sudo ufw allow 51820/udp
sudo ufw allow 3000/tcp
```

---

## 🚀 Startup Instructions & Commands

### 1. Windows 11 Pro Automated Setup & Startup (One-Click Launch)
For Windows 11 Pro environments, automated **Batch scripts** are provided to request administrative elevation (UAC), compile, and boot the application cleanly.

*   **First-Time Setup & Boot (`setup_start.bat`)**:
    - Double-click to run. It automatically requests administrative elevation via UAC.
    - It installs all required packages (`npm install`), builds the production assets, and boots the backend server in production mode (`NODE_ENV=production`).
    - Configured to work seamlessly when the folder is located at `E:\WireGuard Admin\wireguard-vpn-portal`.
*   **Subsequent Quick Boots (`start.bat`)**:
    - Use this for daily starts once the packages are already installed. It rebuilds code and starts the production server instantly with administrative elevation.

---

### 2. Manual CLI Commands (Windows, Linux, macOS)
To manually boot the portal via terminal commands:

```bash
# 1. Clean-install dependencies
npm install

# 2. Boot in development mode (integrated Server & Client)
# Starts the TypeScript backend on 0.0.0.0:3000 using tsx
npm run dev

# 3. Build for production
# Optimizes the React frontend with Vite and bundles the Express server to dist/server.cjs with esbuild
npm run build

# Start production server
npm start
```

---

## 🌐 Bilingual & Default Language Specifications
This management portal supports both **English (EN)** and **Japanese (JP)** seamlessly.

*   **Default Language**: When launching for the first time or after resetting the system, **English (EN)** is loaded as the default language.
*   **Language Switcher**:
    - You can switch to Japanese immediately on the setup or login pages by clicking the **"🌐 日本語に切り替える (JP)"** button in the top right corner.
    - Inside the administrator dashboard, a language toggle (**"🌐 JP"** or **"🌐 EN"**) is available in the header so you can switch layouts at any time. Your preference is persisted in the browser.

---

## 💾 Data Persistence & Config Files

The system persists all database states and portal settings securely using flat JSON files directly on the server host, enabling lightweight, portable backups and migrations without external database servers.

*   `wg-portal-config.json`: Stores core system parameters, including administrator credential hashes, DNS, MTU, Endpoint, Interface definitions, and server private/public keys.
*   `wg-portal-peers.json`: Stores credentials, configurations, and descriptive metadata for all registered peers.

### Resetting System Configuration (Back to Setup Wizard)
After logging in as an administrator, you can click the **"Completely Reset Settings & Database (Return to Setup)"** button in the header. This safely deletes the local configuration files and redirects you to the Setup Wizard instantly, making it easy to migrate to another host or rebuild the environment.

*This software provides secure, high-performance tunneling management for live systems.*
