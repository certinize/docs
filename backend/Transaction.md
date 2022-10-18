# Transaction Flow

This document describes how Certinize initializes a transaction with a wallet provider, the process which comes before the issuance flow.

![transaction-process](https://user-images.githubusercontent.com/65858941/196440691-33b98288-a274-449c-b429-7c39efd7e9bb.jpg)

*Figure 1. Transaction flow diagram*

---

The process goes as follows:

1. The client sends a request to the server.
    1. The request contains a public key.
2. The server receives the request.
    1. It stores the message, public key, and a request ID to a cache or database.
    2. It returns the message and request ID to the client.
3. The client signs the message and sends it back to the server with the request ID.
4. The server receives the signed message and reference ID.
    1. It retreives a public key and message from the cache or database using the reference ID.
    2. It validates the signed message using the retreived message.
    3. If the signed message is valid, it proceeds to the certificate issuance flow.
  
