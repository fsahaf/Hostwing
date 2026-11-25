# Hostwing Privacy Policy

**Effective date:** November 24, 2026  
**Applies to:** Hostwing v1.8.0 (Windows desktop application)  
**Developer:** Farooq Sahaf, Ontario, Canada  
**Contact:** fsahaf@yahoo.com  

---

## 1. Summary

Hostwing is a Windows desktop application that lets you manage remote connections (RDP, SSH, Telnet, SQL Server, file transfer and related tools) from one place. Hostwing runs on your computer. Your connection details, credentials, settings and notes are stored on your device, and the developer does not receive, collect or have access to them.

**Short version:**
- We do not collect your personal information.
- We do not run analytics, telemetry or advertising in the app.
- We do not upload crash reports.
- We do not sell or share your data.
- There is no Hostwing account, cloud service, licence server or automatic update check.
- Hostwing only connects to the systems you tell it to connect to.

---

## 2. Who this policy covers

This policy explains how Hostwing ("the app", "we", "us") handles information when you download, install and use it. It does not cover:
- the websites, servers or services you connect to using Hostwing;
- Microsoft services, including the Microsoft Store, which are governed by the Microsoft Privacy Statement;
- cloud storage or backup providers if you choose to keep the Hostwing document in a synced or backed-up folder;
- Windows components that Hostwing launches on your behalf (for example `mstsc.exe`, `ssh.exe`, the Telnet client or your default web browser), which are part of Windows and have their own behaviour.

---

## 3. Information stored on your device

### 3.1 What Hostwing saves

- **Connection profiles:** display name, host name / IP address / URL, port, protocol, folder, environment badge, optional username, optional RDP gateway, optional database name (SQL), notes, favourite flag, icon, template and protocol-specific options.
- **Credential records:** name, username, folder, credential type, note and an **encrypted password payload**. Passwords are never stored in plain text.
- **Tasks and scripts:** task name, command, arguments, working directory, default target host, notes and the credential name to use.
- **SolarWinds entries:** the SolarWinds server address, the watched node name and the credential name used for authentication.
- **Saved File Transfer sites:** site name, protocol (SFTP, FTP or FTPS), host, port, remote folder and the credential name to use. Passwords are not stored on the site; they live in the referenced credential.
- **Dashboard notes:** the plain text and rich-text (formatted) content you type in the built-in Notes / To-dos pad.
- **Organisation data:** custom folders, folder settings, connection templates, tree expansion state and the name of the global (⭐) credential.
- **Settings:** theme, navigation toggles, the path to the current document and similar UI preferences.
- **A local crash log** (see section 3.4).

### 3.2 Where it is stored

By default:
- `%APPDATA%\Hostwing\connections.json` – The document: connections, credentials, tasks, SolarWinds entries, saved sites, folders, templates, notes and security settings.
- `%APPDATA%\Hostwing\settings.json` – UI preferences and the path to the current document.
- `%APPDATA%\Hostwing\crash.log` – Local crash diagnostics (only created if the app hits an unhandled error).

You can open or save the document to a different path with **Tools ▸ Open Document** / **Save Document As**. Hostwing remembers that path in `settings.json`.

`%APPDATA%` is inside your Windows user profile, so the files are protected by your Windows account and normal file permissions. Other standard users on the same PC generally cannot read them; an administrator, a process running as you, or malware can. If you place the document in a cloud-synced or backed-up folder, that provider stores and may transmit it under its own terms.

### 3.3 How saved passwords are protected

- Saved passwords are encrypted with **AES-256-GCM**, using a random nonce and authentication tag.
- The encryption key is derived from your **master password** with **PBKDF2-SHA256** (210,000 iterations) and a random 16-byte salt.
- Only the salt and a small verification value are stored. The master password itself is never stored and cannot be recovered by the developer.
- If no master password is set, Hostwing cannot save credential passwords. You can still store usernames and connect interactively.
- If you forget the master password, the encrypted passwords cannot be recovered. The rest of the document (host names, usernames, notes and so on) remains readable.
- Passwords are decrypted into memory only when needed for a connection. They are passed to the Windows RDP control, the SSH/Telnet client, the SQL client, the file-transfer library, or (for some management actions) a child PowerShell process through environment variables. They are not written to disk in plain text.
- **The rest of the document is not encrypted.** Host names, usernames, folder names, notes, task commands and dashboard text are stored as readable JSON. They rely on Windows file permissions and, if enabled, disk encryption such as BitLocker. Do not put secrets in notes, task commands or the dashboard pad.

### 3.4 Local diagnostics

If Hostwing hits an unhandled error, it appends a timestamp, the error source and the exception details to `%APPDATA%\Hostwing\crash.log` and shows a dialog. Exception text can contain host names, user names, file paths or SQL error text. The log stays on your device, is never uploaded automatically, and you can delete it at any time.

Hostwing does not persist session history, file-transfer history or SQL query history. In-tab Messages/status text exists only in memory for the life of the tab.

---

## 4. Information we collect

**The app does not collect personal information.** Hostwing has no account system, no analytics, no advertising, no tracking, no telemetry, no automatic crash reporting, no licence activation and no update check. The app does not contact a developer-operated server.

We receive information only if you choose to send it to us, for example by emailing support. In that case we receive your email address, message and any attachments, and we use them to respond and provide support. We keep that correspondence only as long as needed for that purpose, and you can ask us to delete it.

If you install Hostwing from the **Microsoft Store**, Microsoft may collect account, purchase, installation, usage and diagnostic data under the Microsoft Privacy Statement. The developer may receive aggregated, de-identified acquisition and usage reports from Microsoft Partner Center, but does not receive your name, email address or individual usage data from Microsoft unless you provide it. If you download Hostwing from a website, the website host may log requests (such as IP address and user agent) under its own policy.

---

## 5. Network activity

### 5.1 Connections you initiate

Hostwing makes network connections only for the actions you take. Data travels directly between your computer and the remote system you configure. It does not pass through any relay, proxy or server operated by the developer. The remote system, its network, your DNS servers and your network may log connection metadata and traffic according to their own policies.

| Protocol | Default port | What is sent | Protection |
|---|---|---|---|
| RDP | 3389 | Credentials, screen, keyboard, mouse and any redirected devices or clipboard data | TLS/CredSSP depending on the server configuration |
| SSH | 22 | Username, authentication and terminal input/output | Encrypted by SSH |
| Telnet | 23 | Terminal data and any credentials the service prompts for | **Not encrypted by the Telnet protocol** |
| PowerShell remoting | 5985 / 5986 | Commands, output and any credentials the session prompts for, through WinRM | HTTP, or HTTPS if the server is configured for it |
| HTTP / HTTPS / VMware | 80 / 443 | Web requests and responses | TLS for HTTPS; plain HTTP otherwise |
| SQL Server | 1433 | TDS login, queries and result data | Encrypted in transit (`Encrypt=True`), but the server certificate is **not validated** (`Trust Server Certificate=True`) |
| SFTP / SCP | 22 | Username, authentication, file data | Encrypted by SSH. SFTP host keys are accepted without pinning; the fingerprint is shown in the log so you can verify it. |
| FTP / FTPS | 21 | Username, password, file data | FTP is **not encrypted**; FTPS uses TLS |
| SolarWinds (SWIS) | 17774 / 17778 | SWQL query and Basic-auth credentials | HTTPS, but the SolarWinds certificate is **not validated** |
| Hyper-V | — | Opens the local Hyper-V Manager console | No network connection from Hostwing |
| VNC / TeamViewer | — | Stored as placeholder entries; Hostwing does not open a session for them | — |

For SSH and SCP, Hostwing launches the Windows OpenSSH client (`ssh.exe`). Windows may read your SSH configuration and keys from `%USERPROFILE%\.ssh` and may add the remote host key to `%USERPROFILE%\.ssh\known_hosts`. For Telnet, Hostwing launches the Windows Telnet client. For RDP, Hostwing uses the Microsoft RDP ActiveX control embedded in the tab, or `mstsc.exe` when you choose **Open in Windows RDP (all monitors)**. These Windows components may keep their own local caches or settings under your Windows profile.

### 5.2 Web tabs

Web tabs use the Windows `WebBrowser` control (the Internet Explorer engine). The page you open may store cookies, cache and history in your Windows profile, as it would in Internet Explorer. Hostwing does not read or upload that data. **Open in browser** hands the URL to your default browser, which applies its own settings and policies. Windows and Internet Explorer may also contact Microsoft for updates, SmartScreen, certificate checks and similar services according to your Windows settings, independently of Hostwing.

### 5.3 Temporary Windows Credential Manager entry

If you choose **Open in Windows RDP (all monitors)**, Hostwing may create a temporary Windows Credential Manager entry named `TERMSRV/<host>` so that `mstsc.exe` can sign in without prompting. Hostwing deletes that entry when `mstsc.exe` exits or when Hostwing closes, and it never overwrites or deletes an existing entry that was already there. If Hostwing is terminated abnormally, the temporary entry may remain until you remove it in Windows Credential Manager.

### 5.4 No developer-operated services

Hostwing does not contact the developer for updates, licence checks, telemetry, analytics, advertising or crash uploads. There is no Hostwing cloud sync. The app is designed to work entirely on your device and against the systems you configure.

### 5.5 Microsoft Store and website

If you obtain Hostwing from the Microsoft Store, Microsoft processes Store account, purchase, installation and diagnostic data (and purchase data if applicable) under the Microsoft Privacy Statement, and may update the app automatically according to your Store settings. If you obtain it from a website or another distributor, that provider may keep standard web-server logs. Those are separate from the app and are not controlled by Hostwing.

---

## 6. Data on remote systems

When you connect to a remote computer, server or service, you may view, copy or change data on that system, and you may transfer files to or from it. Hostwing does not inspect, store or transmit that data to the developer. You are responsible for:
- having authorization to access the systems you connect to;
- following the privacy, security and legal requirements that apply to the data on them, including regulated data such as health, financial or personal information;
- choosing appropriate protocols and destinations when you copy data to your local computer, the clipboard, a CSV export or a file-transfer destination.

---

## 7. Security

Hostwing is designed to keep your data local and to protect saved passwords as described in section 3. No software can guarantee absolute security. We recommend that you:
- use a strong Windows account password and enable disk encryption such as BitLocker;
- set a strong, unique master password and lock the document when you leave your desk;
- use SSH keys and least-privilege accounts where possible;
- verify SSH/SFTP host-key fingerprints and server certificates for sensitive systems;
- prefer SSH, SFTP, FTPS and HTTPS over Telnet and FTP;
- treat self-signed certificates (accepted for SQL Server and SolarWinds) as suitable only on trusted networks;
- keep Windows and Hostwing up to date;
- protect backups and copies of the Hostwing data folder, because they may contain encrypted passwords and readable connection metadata;
- avoid embedding passwords in task commands, notes or scripts, because those fields are not encrypted;
- delete `crash.log`, exported CSV files and share files when they are no longer needed.

---

## 8. Third parties and open-source components

Hostwing does not share your information with third parties, because the developer does not receive it. The app includes open-source and Microsoft libraries that run locally on your device, including .NET, `Microsoft.Data.SqlClient`, SSH.NET and FluentFTP. These libraries do not send data to the developer.

Other parties may be involved only in ways you choose:
- **Microsoft** — Windows, the RDP/SSH/Telnet/browser components and, if applicable, the Microsoft Store. Microsoft's own privacy statement applies.
- **Your SolarWinds server** — Hostwing sends SWQL queries and Basic-auth credentials directly to the SolarWinds server you configure. That server is operated by you or your organisation.
- **Your website or file host** — if you download Hostwing from a website or keep the document in a synced folder, that provider's policy applies.

We do not sell, rent or trade personal information.

---

## 9. Data retention and deletion

Because your data is stored locally, you control it. To remove it:
- delete individual connections, credentials, tasks, saved sites and notes inside the app;
- delete the document (`connections.json`, or the file you chose with **Save Document As**);
- delete `%APPDATA%\Hostwing\settings.json` and `%APPDATA%\Hostwing\crash.log`;
- uninstall Hostwing, or delete the portable app folder;
- remove any temporary `TERMSRV/<host>` entry from Windows Credential Manager;
- clear the Internet Explorer/Windows web cache if you used web tabs;
- remove Hostwing-related host entries from `%USERPROFILE%\.ssh\known_hosts` if you wish;
- delete exported CSV files, share files, transferred files and any backups or cloud copies.

Uninstalling or deleting the portable app folder does not necessarily delete the data folder, so remove `%APPDATA%\Hostwing` and any custom document path as well. If you forget the master password, the encrypted passwords cannot be recovered.

---

## 10. Children's privacy

Hostwing is a professional tool and is not directed at children under 13 (or the minimum age in your region). The developer does not knowingly collect information from children. Because the app does not collect personal information, there is no child-specific data to delete; if you have a concern, contact us using the details below.

---

## 11. Your rights

The developer does not hold personal information about you through the app, so there is generally nothing for us to access, correct or delete. If you believe we hold information about you (for example, an email you sent to support), contact us and we will respond in line with applicable law, including Canada's Personal Information Protection and Electronic Documents Act (PIPEDA) and, where relevant, the GDPR and similar laws.

If you obtained Hostwing through the Microsoft Store, Microsoft controls Store account and purchase data. You can exercise access, correction and deletion rights for that data through your Microsoft account and the Microsoft Privacy Statement.

---

## 12. International users

The developer is based in Canada. The app does not transfer your app data to the developer. When you connect to a remote system, traffic may cross borders depending on where that system and your network are located; your organisation's rules and the remote system's policies apply. Microsoft Store data is handled by Microsoft under its own terms.

---

## 13. Changes to this policy

We may update this policy when the app changes, for example if a feature is added that sends data off your device. The effective date at the top shows the latest version. Material changes will be described in the app's release notes or on our website.

---

## 14. Contact

Questions about this policy can be sent to:

**Farooq Sahaf**  
Ontario, Canada  
Email: fsahaf@yahoo.com

---

### Appendix A — Microsoft Store disclosure summary

| Store question | Answer |
|---|---|
| Does the app collect personal information? | No, not by the developer. |
| Does the app collect analytics, telemetry or advertising data? | No. |
| Does the app upload crash reports? | No. A local `crash.log` is kept on the device. |
| Does the app require an account? | No. |
| Does the app sell or share data? | No. |
| What is stored locally? | Connection profiles, credential records (passwords encrypted), tasks/scripts, saved sites, notes, settings and a local crash log. |
| Is data transmitted off the device? | Only to endpoints you configure, when you connect. There are no developer-operated servers. |
| Is data encrypted in transit? | Depends on the protocol: TLS/SSH for RDP, SSH, SFTP, FTPS, HTTPS and SQL. SQL Server and SolarWinds certificates are not validated. Telnet and FTP are plain text. |
| Is data encrypted at rest? | Saved passwords use AES-256-GCM with a PBKDF2-derived key. Other document fields rely on Windows file permissions and disk encryption. |
| How can users delete their data? | Delete entries in the app, delete the document and `%APPDATA%\Hostwing`, uninstall the app, and remove any exported or backed-up copies. |
| Privacy policy URL | `https://fsahaf.github.io/Hostwing/PRIVACY.html` |
| Contact | fsahaf@yahoo.com |

---

*This policy is published at `https://fsahaf.github.io/Hostwing/PRIVACY.html`.*