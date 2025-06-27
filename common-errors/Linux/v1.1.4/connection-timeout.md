# Datagram Node Error: Connection Timeout / TLS Certificate Panic

**Problem Found and reported by:** aetrna | drinkyouroj | nightwind
**Resolved by:**  aetrna

---

## ❗ Problem

This issue may appear when running the `datagram` CLI binary — especially on fresh servers or VPS environments without a properly configured certificate authority.

Example error:

```bash
./datagram-cli-x86_64-linux run -- -key eb24c9734859e1ff0473fef
```

Output:

```
App version: 1.1.4
Conference version: 1.1.4
panic: error connect: read tcp 192.x.x.x:56812->18.163.22.61:5671: read: connection reset by peer

goroutine 19 [running]:
main.cmdEnv({0x7ffeb1c0d961, 0x20}, 0xa519)
        /home/runner/work/datagram-conference-cli/datagram-conference-cli/main.go:252 +0x985
main.runProcessManager.func1()
        /home/runner/work/datagram-conference-cli/datagram-conference-cli/main.go:107 +0x68
created by main.runProcessManager in goroutine 1
        /home/runner/work/datagram-conference-cli/datagram-conference-cli/main.go:104 +0x148
Error executing binary: exit status 2
```

---

## ✅ Solution

The issue is caused by the client system **rejecting the Datagram server’s TLS certificate** due to a **missing Root CA trust**.

To fix it:

### 1. Shutdown any nodes on related services (if running), and use `screen` for convenience

Start your screen session:

```bash
screen -r your_screen_name
```

---

### 2. Install required tools and fetch the server certificate

```bash
apt update && apt install -y wget curl ca-certificates openssl
```

Extract the self-signed certificate:

```bash
openssl s_client -connect 18.163.22.61:5671 -showcerts </dev/null 2>/dev/null \
| awk '/-----BEGIN CERTIFICATE-----/,/-----END CERTIFICATE-----/{ print }' \
> datagram-ca.crt
```

---

### 3. Trust the certificate system-wide

Move and install it into your certificate store:

```bash
cp datagram-ca.crt /usr/local/share/ca-certificates/
update-ca-certificates
```

---

### 4. Verify installation

Check for errors:

```bash
openssl s_client -connect 18.163.22.61:5671 -showcerts
```

Make sure you **do not see** this message:

```
verify error:num=19:self-signed certificate in certificate chain
```

---

### 5. Re-run the node

```bash
./datagram-cli-x86_64-linux run -- -key YOUR_LICENSE_KEY
```

---

## 🧠 Why This Happens

The error:

```
read: connection reset by peer
```

was caused by a **failed TLS handshake** between the client and `rbq-prod.datagram.network:5671`.

### Root Cause:

* The server uses a **self-signed root certificate** (`TLSGenSelfSignedRootCA`).
* Many Linux systems **do not trust self-signed certificates by default**.
* This results in the Go-based binary failing with a misleading connection reset error.

---

## 🔧 Why This Solution Works

By manually extracting and trusting the server’s Root CA:

* You enable successful TLS handshakes.
* The Go TLS library will now accept the connection.
* The binary is able to proceed with the node startup successfully.

---

## 📝 Notes

* This issue is **environment-dependent**.
* It primarily affects **clean VPS installs** or **custom Linux environments**.
* Once trusted, the node can run normally even on reboots or under `systemd`.

---

## ⚠️ Important Note

If this error occurs across **many different CLI users**, the root cause is **likely server-side** — either a misconfiguration of the TLS certificate or a missing CA bundle being served improperly.

> In such cases, the Datagram tech team should investigate the certificate deployment and update the TLS setup for better compatibility across platforms.




