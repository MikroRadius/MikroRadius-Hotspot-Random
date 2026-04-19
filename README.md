# MikroRadius Captive Portal with Randomized MAC Detection

> A modern, user-friendly captive portal for MikroTik hotspots featuring a QR code scanner and advanced randomized MAC address detection to ensure only genuine devices can authenticate.

This is the official MikroRadius captive portal, designed for anyone to use. It enhances the standard MikroTik login experience with a clean interface, a built-in QR code scanner for easy voucher redemption, and a robust system to detect and guide users who have device privacy features (randomized MAC addresses) enabled. The portal is built with HTML, CSS, and JavaScript (including MD5 hashing for security) and is ready to be deployed on your MikroTik router.

**Key Features:**

*   **QR Code Scanner**: Allows users to scan a QR code voucher directly from the login page, streamlining the authentication process.
*   **Randomized MAC Detection**: Detects when a user's device is using a private, randomized MAC address and provides clear, OS-specific instructions (iPhone, Android, Windows) on how to disable it. This is crucial for preventing login issues and ensuring a consistent user experience.
*   **Modern & Responsive UI**: A clean, mobile-friendly design that works seamlessly across all devices.
*   **MikroTik-Ready**: Built with MikroTik's variable system (`$(...)`), making it a drop-in replacement for the default hotspot pages.
*   **Includes All Necessary Pages**: Provides a complete set of hotspot pages, including `login.html`, `logout.html`, `status.html`, `error.html`, and more.

## 📂 Repository Structure

The repository contains the core files needed for the captive portal:

*   **`login.html`**: The main login page with the voucher form, QR scanner, and MAC detection guide.
*   **`alogin.html`**: The page shown after a successful login.
*   **`logout.html`**: The page shown after a user logs out.
*   **`status.html`**: Displays the current user's connection status.
*   **`error.html`**: Generic error page.
*   **`redirect.html`** & **`rlogin.html`**: Used in the authentication redirect flow.
*   **`radvert.html`**: Page for displaying advertisements (if configured).
*   **`md5.js`**: JavaScript library used for password hashing during the login process.
*   **`xml/`**: Directory containing `WISPAccessGatewayParam.xsd` and XML versions of the HTML pages for broader compatibility.

## 🚀 Setup Guide: Deploying on MikroTik

Follow these steps to replace your MikroTik hotspot's default pages with the MikroRadius portal.

### Prerequisites

*   A MikroTik router with an active and configured Hotspot.
*   Access to your router via **Winbox** or **SSH**.
*   FTP access to your router to upload files.

### Step 1: Upload Portal Files to MikroTik

1.  Connect to your MikroTik router using an FTP client (like FileZilla). Use the same credentials you use for Winbox/SSH.
2.  Navigate to the root directory of the router's filesystem.
3.  If a directory named `hotspot` does not exist, create it.
4.  Upload **all** the `.html` files and the `md5.js` file from this repository into the `hotspot` directory.
5.  **Important:** Upload the `xml/` folder and its contents into the `hotspot` directory as well. The final structure should look like this:
/hotspot/
├── alogin.html
├── error.html
├── login.html
├── logout.html
├── md5.js
├── radvert.html
├── redirect.html
├── rlogin.html
├── status.html
└── xml/
├── WISPAccessGatewayParam.xsd
├── alogin.html
├── error.html
├── flogout.html
├── login.html
├── logout.html
├── redirect.html
└── rlogin.html

### Step 2: Configure the Hotspot to Use the New Files

1.  Open **Winbox** and connect to your router.
2.  Go to **IP** → **Hotspot**.
3.  Select your Hotspot server from the list and double-click it, or go to the **Server Profiles** tab and edit the profile being used by your server.
4.  Navigate to the **HTML Directory** tab.
5.  Check the box for **Use HTML directory**.
6.  In the **Directory** field, enter the name of the folder you created: `hotspot`
7.  Click **OK**.

### Step 3: Configure the Walled Garden (Crucial for QR Scanner)

The QR code scanner requires access to `https://qr.mikroradius.com` to function correctly. You must add this domain to your Hotspot's Walled Garden to allow unauthenticated users to reach it.

**Add this Walled Garden rule via the command line (SSH or Terminal in Winbox):**

```bash
/ip hotspot walled-garden ip
add action=accept comment="MikroRadius QR Code Scanner" disabled=no dst-host=https://qr.mikroradius.com
