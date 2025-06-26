# Datagram Node Troubleshooting & Alpha Testnet Guide

This repository is a collaborative knowledge base for troubleshooting and understanding the Datagram Node — especially during the Alpha Testnet phase. It includes real-world errors, solutions, and FAQs compiled by community node runners.

## Table of Contents

- [Overview](#overview)
- [Getting Started](#getting-started)
- [Common Errors](#common-errors)
- [Alpha Testnet FAQ](#alpha-testnet-faq)
- [Setup Guides](#setup-guides)
- [Useful Commands](#useful-commands)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

This documentation aims to help node runners troubleshoot installation issues, runtime errors, and behavior during the early test phases of Datagram. Content is updated as new issues arise and fixes are found.

---

## Getting Started

If you're just beginning:
1. Download the latest node binary from the official source.
2. Ensure your system meets the requirements.
3. Follow the [setup guide](setup-guide/linux.md) for your platform.

---

## Common Errors

We have documented common problems and their fixes, such as:
- Invalid license key issues
- TMP directory or cross-device link errors
- Frequent reconnecting
- No points showing

See [common-errors/](common-errors/) for detailed solutions.

---

## Alpha Testnet FAQ

Everything you need to know about:
- Uptime points system
- Referral rules
- Mobile/VPN support
- Node count per account

View the full FAQ in [faq/alpha-testnet.md](faq/alpha-testnet.md)

---

## Setup Guides

Platform-specific installation steps:

- [Linux](setup-guide/linux.md)
- [Windows](setup-guide/windows.md)
- [macOS](setup-guide/mac.md) 

---

## Useful Commands

Basic system and node commands used for debugging:

```bash
# Check node service status
systemctl status datagramd

# View recent logs
journalctl -u datagramd -n 50 --no-pager

# Restart node
systemctl restart datagramd
```

---

## Contributing

We welcome contributions! If you've encountered a new error or have a clearer solution:

1. Fork this repo
2. Add your changes (in Markdown)
3. Submit a pull request

Consistency in formatting and language is appreciated.

---

## License

This documentation is open-source under the [MIT License](LICENSE).



