# MuleSoft One-Way TLS Integration

This repository demonstrates a **practical implementation of one-way TLS security between Mule applications**.

The objective is to illustrate how Mule services can establish secure communication using **keystore and truststore configuration**, while validating the TLS handshake at the protocol level.

This lab reproduces a simplified integration scenario often encountered in enterprise environments.

Full technical explanation available here:  
https://medium.com/@amsatambengue/end-to-end-one-way-tls-security-between-mule-applications-8916dd895ab4


---

# Architecture Overview

The integration follows a simple client-server pattern.

Postman
↓ HTTPS
Mule Client (Truststore)
↓ HTTPS
Mule Server (Keystore)

- **Postman**
  - acts as an external HTTPS client to trigger the end-to-end flow

- **Mule Client**
  - performs an outbound HTTPS call
  - validates the server certificate using a **truststore**

- **Mule Server**
  - exposes a secure HTTPS endpoint
  - presents its certificate during the TLS handshake
  - uses a **PKCS12 keystore**

---

# Repository Structure

oneway-tls-server/
Mule application exposing an HTTPS endpoint secured with TLS

oneway-tls-client/
Mule application calling the server over HTTPS

The two applications simulate a **secure Mule-to-Mule integration**.

The keystore and truststore used in this lab are located in each application's src/main/resources directory.
---

# Prerequisites

To reproduce the example locally:

- Anypoint Studio 7.x
- JDK 17
- OpenSSL
- Postman
- Basic knowledge of Mule connectors (HTTP Listener, HTTP Request)

---

# Running the Example

## 1 Start the Server Application

Run the Mule application: oneway-tls-server
The server exposes: https://localhost:8081/health


The HTTPS listener uses a **PKCS12 keystore containing the server certificate**.

---

## 2 Start the Client Application

Run the Mule application: oneway-tls-client

The client exposes: https://localhost:8082/test
This endpoint triggers an outbound HTTPS request to the server.


---

## 3 Validate End-to-End TLS Communication

Send a request to the client endpoint: https://localhost:8082/test

Expected response:

{
"app": "oneway-tls-server",
"status": "ok"
}


This confirms that:

- the HTTPS connection is established
- the server certificate is validated by the client
- encrypted communication is successfully negotiated


---

## Testing the Endpoints

Once both applications are running, you can verify the integration using any HTTP client such as Postman or curl.

Server endpoint:

https://localhost:8081/health

Client endpoint:

https://localhost:8082/test

---


# Inspecting the TLS Handshake (Optional)

The TLS handshake can be inspected using OpenSSL:
openssl s_client -connect localhost:8081 -servername localhost


Typical output:
Protocol : TLSv1.3
Cipher : TLS_AES_256_GCM_SHA384


This confirms that the communication uses a **modern TLS configuration**.

---

# What This Repository Demonstrates

- Practical configuration of **keystore and truststore in MuleSoft**
- Implementation of **one-way TLS authentication**
- Secure **Mule-to-Mule communication**
- TLS handshake inspection and validation

---

# Author

Amsata Mbengue  
MuleSoft Integration Engineer

Medium:  
https://medium.com/@amsatambengue
