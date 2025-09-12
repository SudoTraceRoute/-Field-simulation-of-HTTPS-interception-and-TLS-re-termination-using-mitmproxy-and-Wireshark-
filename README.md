**📡 Field-simulation-of-HTTPS-interception-and-TLS-re-termination-using-mitmproxy-and-Wireshark**
The illusion of end-to-end encryption collapses under the weight of a trusted root certificate and a transparent forward proxy.


---

## 🧭 Project Overview

This repository demonstrates the implementation and analysis of TLS interception in a local environment, simulating enterprise-grade HTTPS decryption and passive DPI workflows. 
A rogue CA is deployed to an Android-based client (v8.0) to facilitate trust in an intercepting MITM proxy. 
Traffic is then programmatically correlated with a lower-level packet capture to analyze the handshake lifecycle, certificate forging behavior, and decrypted HTTP-layer payloads.

The project focuses on **protocol-level accuracy** and **operational parity** with real-world TLS re-termination systems (e.g., Palo Alto SSL Forward Proxy, Blue Coat SSL Intercept). 
Emphasis is placed on correlation between:
- Raw packet flow (Wireshark)
- Application-level session flows (mitmproxy)

No GUI walkthroughs or basic tutorials are provided or intended.

---

