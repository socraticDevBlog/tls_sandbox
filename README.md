# tls sandbox project

totally vibe coding this one 🤖🎨

## prompt

"im looking for a sandbox project to understand how TLS works?"

## copilot

Sandbox Project: Understanding How TLS Works
This project will help you:

Set up an HTTPS server with a self-signed TLS certificate.
Establish a secure connection between a client and the server.
Understand key TLS concepts like certificates, private keys, and encryption.

### Steps for the Project

1. Generate a Self-Signed TLS Certificate
Use openssl to generate a self-signed certificate.

```bash
# Generate a private key
openssl genrsa -out server.key 2048

# Generate a self-signed certificate
openssl req -new -x509 -key server.key -out server.crt -days 365
```

This will create two files:

- server.key: The private key.
- server.crt: The self-signed certificate.

2. Create an HTTPS Server
Use Python's http.server and ssl modules to set up an HTTPS server.

```python
import ssl
from http.server import HTTPServer, SimpleHTTPRequestHandler

# Configure the HTTPS server
server_address = ('localhost', 4443)
httpd = HTTPServer(server_address, SimpleHTTPRequestHandler)

# Load the certificate and private key
context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
context.load_cert_chain(certfile="server.crt", keyfile="server.key")

# Wrap the server socket with SSL
httpd.socket = context.wrap_socket(httpd.socket, server_side=True)

print("HTTPS server running at https://localhost:4443")
httpd.serve_forever()
```

3. Create an HTTPS Client
Use Python's requests library to create a client that connects to the HTTPS server.

```python
import requests

# Disable warnings for self-signed certificates (for sandbox purposes only)
import urllib3
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Make a GET request to the HTTPS server
response = requests.get("https://localhost:4443", verify=False)
print("Server response:", response.text)
```

4. Run the Project

Start the HTTPS server:
```
python server.py
```

Run the HTTPS client in another terminal:
```
python client.py
```

You should see the server's response printed in the client's terminal.

### Concepts You'll Learn
TLS Certificates:

Understand the role of certificates and private keys.
Learn the difference between self-signed certificates and certificates signed by a Certificate Authority (CA).
Encryption:

How TLS encrypts data to ensure confidentiality.
Authentication:

How TLS uses certificates to authenticate the server (and optionally the client).
Data Integrity:

How TLS ensures that data is not tampered with during transmission.

### Next Steps
Add Mutual Authentication:

Configure the server to require a client certificate and modify the client to provide one.
Use a Certificate Authority (CA):

Create a local CA to sign certificates instead of using a self-signed certificate.
Explore TLS Versions:

Experiment with different TLS versions (e.g., TLS 1.2, TLS 1.3) and understand their differences.
Integrate with a Real Application:

Use TLS in a real-world application, such as a RESTful API or a web application.

_This sandbox project is an excellent starting point to understand the basics of TLS and how it secures communication over the internet._

