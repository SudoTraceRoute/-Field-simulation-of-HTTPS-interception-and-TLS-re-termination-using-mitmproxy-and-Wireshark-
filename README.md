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


## 🧱 System Architecture

```text
[Android Client]
    ↓ (TLS over Proxy, trusted mitm CA)
[Linux Host running mitmproxy:8080]
    ↓ (TLS re-initiation)
[Remote Web Servers (HTTPS)]
Client: Android 8.0 (certificate manually installed)

Proxy: mitmproxy running on PC, port 8080, regular mode

Packet capture: Wireshark (capture filter on Android IP)

Transport: Wi-Fi (WPA2 PSK; prior decryption simulation covered in previous repo)

**⚙️ Configuration & Command Reference**
bash
Copy code
# Obtain local IP (for proxy endpoint configuration)
ifconfig

# Launch mitmproxy in explicit mode on TCP 8080
mitmproxy --mode regular --listen-port 8080

# Capture flows to log file (optional)
mitmdump -w my_log.log

# Inspect previously captured flows
mitmdump -r my_log.log

# Start Wireshark with interface bound to the active adapter
# Filter set: ip.addr == <android_client_ip>
Certificate Installation (Phone)
Cert transferred via Bluetooth

**Alternatively, run temporary HTTP server for pull:**

# Option A: Python HTTP server
python3 -m http.server 8081

# Option B: Apache (pre-installed)
sudo service apache2 start
Client manually installs .pem or .crt file under trusted credentials (user)

**🧪 Protocol-Level Artifact Index**
✅ TCP 3-Way Handshake (Phone → Proxy)
SYN / SYN-ACK / ACK exchange confirms initial TCP socket creation

Critical for establishing reliable Layer 4 channel prior to TLS negotiation

**✅ TLS Handshake Lifecycle**
Client Hello: Includes SNI, cipher suites, TLS extensions

Server Hello: Proxy-generated, forged cert with issuer = mitmproxy

Certificate message: X.509 with full chain; subject CN = target domain

Finished: Indicates session encryption state established

Captured via:

wireshark

tls.handshake.type == 1  # Client Hello
tls.handshake.type == 2  # Server Hello
tls.handshake.type == 11 # Certificate

**✅ DNS Resolution Preceding TLS**

DNS queries for FQDNs (e.g., www.google.com) resolved before TLS start

Observed via UDP port 53 responses with A/AAAA records

**wireshark**

**DNS**
Key field: Answers → Address: x.x.x.x
**
✅ Encrypted Application Data**
Post-handshake records show TLS content-type 23

Decryption not visible in Wireshark (as expected); validated via mitmproxy

**wireshark**

tls.record.content_type == 23
🪓 Decrypted Payload Inspection (via mitmproxy)
🔍 Flow List View
Screenshot: screenshots/mitmproxy-flow-list.png

Each flow represents a successful TLS re-termination and HTTP-layer capture

Visible endpoints, status codes, and method types

**🔓 Individual Flow: Request**
Screenshot: screenshots/mitmproxy-request.png

GET /news/photo.jpg HTTP/1.1

Headers: User-Agent, Host, Accept, Cookie

Proves L7 visibility post-TLS termination

🔐 Individual Flow: Response
Screenshot: screenshots/mitmproxy-response.png

200 OK, Content-Type: image/jpeg

Content-length and response body (truncated for brevity)

Confirms bidirectional inspection capability

**📸 Wireshark Screenshot Index**
Screenshot	Description
tcp-handshake.png	SYN → SYN/ACK → ACK between Android and proxy
client-hello.png	TLS ClientHello with SNI and cipher spec
server-hello-cert.png	TLS ServerHello + forged certificate (issuer = mitmproxy)
dns-query.png	A/AAAA record DNS resolution for target FQDN
tls-encrypted-data.png	TLS application data packets (post-handshake)

🧠 Observations
The Android device accepted a proxy-forged certificate chain due to explicit CA installation.

TLS handshake proceeded without errors, despite termination and re-initiation.

mitmproxy seamlessly forged certs per domain, maintaining subject CN/SAN integrity.

DNS remains observable pre-TLS unless DoH/DoT is enforced by the client.

No SNI encryption observed (as expected on Android 8.0 with TLS 1.2 defaults).

🛑 Limitations / Notes
TLS 1.3 with Encrypted Client Hello (ECH) is not supported in this scenario.

Modern apps using certificate pinning will fail to complete handshakes.

Android > 10 restricts CA trust scope to system-level CAs unless user-deployed apps explicitly allow them.

🧩 Conclusion
This repo simulates the operational logic behind enterprise-grade TLS interception mechanisms using open-source tools in a lab-controlled environment. It bridges the gap between network transport capture (Wireshark) and decrypted application flow inspection (mitmproxy), aligning with practices used in DLP, APT sandboxing, and threat hunting infrastructure.
