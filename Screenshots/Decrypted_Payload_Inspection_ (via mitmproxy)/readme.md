**🖼️ Screenshot: mitmproxy Flow List**

📄 Explanation:

This screenshot shows mitmproxy's active session flow list, capturing multiple HTTPS requests from the Android client. Each entry represents a decrypted HTTP transaction intercepted over a TLS session. This confirms successful TLS interception and application-layer visibility.
📌 What it proves:
    • Real-time decryption of HTTPS
    • Multiple sessions successfully intercepted
    • MITM proxy functioning as expected

**🖼️ Screenshot: Decrypted HTTP Request View**

📄 Explanation:

This view shows a decrypted HTTP GET request intercepted by mitmproxy. Key headers such as Host, User-Agent, and Accept are visible, demonstrating the proxy’s ability to inspect plaintext application data over a TLS channel.
📌 What it proves:
    • Full HTTP payload visibility
    • TLS has been successfully terminated by the proxy
    • Traffic originated from Android and passed through mitmproxy

**🖼️ Screenshot: Decrypted HTTP Response View**

📄 Explanation:

The decrypted HTTP response from the origin server, relayed by mitmproxy to the Android client. This demonstrates that the proxy is operating bidirectionally, relaying and inspecting both request and response streams within the same session.
📌 What it proves:
    • Bi-directional traffic capture (request + response)
    • Integrity of the interception — no breakage
    • Proxy acts as a full TLS-terminating man-in-the-middle
