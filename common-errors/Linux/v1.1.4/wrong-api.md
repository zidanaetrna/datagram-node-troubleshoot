# Datagram Node Error: Wrong API Key Used (Wrong Key Source)

**Problem Found and Reported by:** aetrna | drinkyouroj | nightwind

---

## ❗ Problem

Some users are mistakenly using an **API key from the Account > APIs page** instead of the correct **License key from Wallet > Licenses**, which leads to silent failure of the node.

Example command run by user:

```bash
datagram-cli run -- -key org_d17bahkqnn6s73crlvcg:a0En6/iqvWF4...
```

CLI output:

```
App version: 1.1.4
Downloading...
Conference version: 1.1.4
Running...
```

This appears normal, but the node **does not actually connect to the dashboard**.

---

## ✅ Solution

If your key starts with `org_`, you are **definitely using the wrong API key**.

You should:

1. Go to **Wallet > Licenses** from your Datagram dashboard.
2. Copy the **License Key** instead of the API key.
3. Re-run the node using the correct license key:

```bash
datagram-cli run -- -key YOUR_LICENSE_KEY
```

This should allow the node to fully register and appear in your dashboard.

---

## 🧠 Why This Happens

* The Datagram CLI currently **does not differentiate between API keys and license keys** at runtime.
* Even with the wrong key, the app outputs normal logs like:

```
App version: 1.1.4
Downloading...
Conference version: 1.1.4
Running...
```

* This **misleads users into thinking their node is running correctly**, even though the backend rejects the connection due to invalid license.

---


## 📝 Notes

* License keys are only available from **Wallet > Licenses** — never from `account > APIs`.
* A valid license key does **not** start with `org_` — if it does, you're using an API key.
* Until error-handling is improved, double-check that you are using the **license key** when running nodes.



