**🖼️ 1. TCP 3-Way Handshake (Phone to Proxy)**
📄 Explanation:
This screenshot captures the classic TCP 3-way handshake between the Android client and the local MITM proxy. The client initiates a connection with a SYN, the proxy responds with SYN-ACK, and the client completes the handshake with an ACK. This establishes a reliable TCP session, required before initiating the TLS handshake over port 8080.
📌 Protocols involved:
    • TCP (Layer 4)
📌 Purpose:
    • Establishes a connection-oriented, full-duplex communication channel.

**🖼️ 2. TLS Client Hello**
📄 Explanation:
The client initiates the TLS handshake by sending a Client Hello, advertising supported TLS versions, cipher suites, and extensions. It includes the SNI (Server Name Indication), allowing the proxy to identify the intended destination domain (e.g., www.google.com). This packet begins the encrypted session setup.
📌 Protocols involved:
    • TLS Handshake over TCP
📌 Purpose:
    • Begins session negotiation for encrypted communication.

**🖼️ 3. TLS Server Hello + Certificate (Forged by mitmproxy)**
📄 Explanation:
The MITM proxy responds with a Server Hello and presents a dynamically forged X.509 certificate. The certificate's issuer field identifies mitmproxy as the signing authority, proving interception. This mimics legitimate server behavior while allowing the proxy to terminate and inspect TLS traffic.
📌 Protocols involved:
    • TLS Handshake, X.509 Certificates
📌 Purpose:
    • Establishes trust and encryption parameters between client and proxy.

**🖼️ 4. DNS Query and Response**
📄 Explanation:
The client queries the configured DNS resolver to resolve the target domain (e.g., www.google.com). The A record in the answer section returns the real server's IP address. This occurs before TLS begins, making it a critical observable point in intercepted traffic.
📌 Protocols involved:
    • DNS (UDP, port 53)
📌 Purpose:
    • Resolves domain names to IP addresses for connection setup.

**🖼️ 5. TLS Encrypted Application Data**
📄 Explanation:
This view shows Application Data TLS records (Content Type 23), indicating that the TLS handshake has completed and encrypted traffic is flowing. At this point, plaintext HTTP data is inaccessible via Wireshark unless decrypted externally, e.g., by mitmproxy.
📌 Protocols involved:
    • TLS Record Layer (Application Data)
📌 Purpose:
    • Transfers encrypted application-layer payloads (e.g., HTTP requests/responses).
