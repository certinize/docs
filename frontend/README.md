# Front-end Specification

## Summary

This specification describes the front-end feature integration and underlying technology stack of the project. Many features related to the project, including architectural style, deployment process, and UI/UX design are not described here and left to the reader’s interpretation.

## Table of Contents

- [Technologies](#technologies)
    - [Front-end Client](#front-end-client)
- [Logic Flow](#logic-flow)
    - [User Authentication and Authorization](#user-authentication-and-authorization)
    - [E-Certificate Management](#e-certificate-management)
        - [Creation](#creation)
        - [Issuance](#issuance)
        - [Verification](#verification)

## Technologies

This section specifies the technologies necessary for the project’s completion. For now, it does not include third-party frameworks and libraries which are subject to change.

### Front-end Client

- JavaScript
- Node.js

## Logic Flow

This section describes the general system use case, not including features that deviate from the purpose of the project.

Note: The API endpoints mentioned in this section are still in development. You will not find any documentation pertaining to their usage besides this specification.

### User Authentication and Authorization

- Users should be able to gain access to the system using their [Solana Wallet](https://solana.com/ecosystem/explore?categories=wallet).
- The system should check if the user already has an account on the system. To do so, the user’s wallet address should be attached as a path parameter to the `/users` endpoint and an HTTP GET request should be made.

Example request:

```
/users/9ZNTfG4NyQgxy2SWjSiQoUyBPEvXT2xo7fKc5hPYYJ7b
```

Regardless whether or not the user already has an account, the `/users/auth` endpoint should return a JSON object containing an API Key (UUID).

Example response body:

```json
{
  "api-key": "78c04a31-6c85-471c-a8cc-da5b547baaae"
}
```

The system should be able to retrieve the API Key during the entirety of the user session.

> Tip: For methods on how to save the API Key, see the answers to this Stack Overflow question: [https://stackoverflow.com/questions/51916453/how-to-store-generated-api-keys-in-react](https://stackoverflow.com/questions/51916453/how-to-store-generated-api-keys-in-react)

## E-Certificate Management
- [Creation](#creation)
- [Issuance](#issuance)
- [Verification](#issuance)

### **Creation**

Users should be able to create or generate an e-Certificate using the process described in this section.

- Users should be able to create an e-Certificate template using a web-based certificate builder.

- The e-Certificate builder should prompt the user to choose between two (2) options:

    - Upload and use an e-Certificate template and a [TrueType Font (TTF)](https://en.wikipedia.org/wiki/TrueType) file.
    - Use an already existing, on-premise e-Certificate template from a template gallery and a TFF file from a font menu.

    <br>

    >At this point, the selected e-Certificate template should only require the name of the recipient and the issuance date. In other words, it should already contain the title, presentation line, description, signatory, signature, etc.

- To get a list of e-Certificate templates, make an HTTP GET request to the `/templates` endpoint.

  Example response body:
  
  ```json
  {
    "templates": [
      {
        "template_id": "900bb16f-77a8-47d0-82a4-5a7b10b34296",
        "template_url": "http://bucket.s3.provider.com/template.png"
      },
      ...
    ]

  }
  ```

- The e-Certificate builder should prompt the user to specify the position where the name of the recipient and the issuance date should be placed.

    > Tip: This can be achieved by using two reticles or cross-hairs you can drag across the certificate.

- The e-Certificate template configuration should be saved by making an HTTP POST request to the `/configurations` endpoint.

    The HTTP POST request body should be in raw [JavaScript Object Notation (JSON)](https://en.wikipedia.org/wiki/JSON) format and it should contain three (3) key-value pairs:

        - recipient_name: [position and font size]
        - issuance_date: [position and font size]
        - template_id: [ID assigned to the e-Certificate template]

    Example request body:

    ```json
    {
      "recipient_name": {
        "position": {
          "x": 960,
          "y": 540
        },
        "font_size": 24
      },
      "issuance_date": {
        "position": {
          "x": 130,
          "y": 950
        },
        "font_size": 12
      },
      "template_id": "900bb16f-77a8-47d0-82a4-5a7b10b34296"
    }
    ```

    In addition, the request should contain the [X-API-Key header](https://en.wikipedia.org/wiki/API_key) and the user’s corresponding API Key.

    The `/configurations` endpoint should return a JSON object containing the information about the saved e-Certificate template configuration.

    Example response body:

    ```json
    {
      "template_config_id": "cbddb991-41f3-44b7-a377-f5d98755e881",
      "template_id": "900bb16f-77a8-47d0-82a4-5a7b10b34296",
      "recipient_name": {
        "position": {
          "x": 960,
          "y": 540
        },
        "font_size": 24
      },
      "issuance_date": {
        "position": {
          "x": 130,
          "y": 950
        },
        "font_size": 12
      }
    }
    ```

### **Issuance**

Users should be able to issue an e-Certificate using the process described in this section.

- When generating e-Certificates, the interface should prompt the user to select an e-Certificate template configuration from a template gallery.

- To retrieve a list of e-Certificate template configurations, the client should make an HTTP GET request to the `/configurations` endpoint.

    In addition, the request should contain the X-API-Key header and the user’s corresponding API Key.

    The `/configurations` endpoint should return a JSON object containing a list of certificate configuration IDs.

    Example response body:

    ```json
    {
      "configurations": [
        {
          "template_config_id": "cbddb991-41f3-44b7-a377-f5d98755e881",
          "template_id": "900bb16f-77a8-47d0-82a4-5a7b10b34296",
          "recipient_name": {
            "position": {
              "x": 960,
              "y": 540
            },
            "font_size": 24
          },
          "issuance_date": {
            "position": {
              "x": 130,
              "y": 950
            },
            "font_size": 12
          }
        },
        ...
      ]
    }
    ```

- A specific e-Certificate template configuration should be retrieved by making an HTTP GET request to the `/configurations` endpoint by attaching the template configuration ID as path parameter, like so:

    ```
    /configurations/cbddb991-41f3-44b7-a377-f5d98755e881
    ```

    In addition, the request should contain the X-API-Key header and the user’s corresponding API Key.

    The `/configurations` endpoint should return a JSON object containing three (3) key-value pairs:

        - recipient_name
        - issuance_date
        - template_config_id

    Example response body:

    ```json
    {
      "recipient_name": {
        "position": {
          "x": 960,
          "y": 540
        },
        "font_size": 24
      },
      "issuance_date": {
        "position": {
          "x": 130,
          "y": 950
        },
        "font_size": 12
      },
      "template_config_id": "cbddb991-41f3-44b7-a377-f5d98755e881"
    }
    ```

- The interface should prompt the user to enter the recipient name, email address, and Solana wallet address.
- To generate an e-Certificate(s), the entered information should be sent to the `/certificates` endpoint via an HTTP POST request.

    The HTTP POST request body should be a JSON object containing the e-Certificate template configuration ID and the information the user entered.

    Example request body:

    ```json
    {
      "template_config_id": "cbddb991-41f3-44b7-a377-f5d98755e881",
      "recipients": [
        {
          "recipient_name": "Juan Cruz",
          "email_address": "juan_cruz@email.com",
          "wallet_address": "9ZNTfG4NyQgxy2SWjSiQoUyBPEvXT2xo7fKc5hPYYJ7b"
        }
      ]
    }
    ```

    Recipient information should be appended as an object to the recipients list. Each list element/object should hold recipient information, including recipient name, email address, and wallet address.

    The `/certificates` endpoint should return a JSON object containing six (6) key-value pairs:

        - template_config_id
        - template_url
        - recipient_name_position
        - issuance_date_position
        - issuance_date
        - recipients

    Example response body:

    ```json
    {
      "template_id": "cbddb991-41f3-44b7-a377-f5d98755e881",
      "template_url": "https://bucket.s3.objectstorageprovider.com/certificate_template.jpg",
      "recipient_name_position": {
        "x": 960,
        "y": 540
      },
      "issuance_date_position": {
        "x": 130,
        "y": 950
      },
      "issuance_date": "2022-01-31",
      "recipients": [
        {
          "recipient_name": "Juan Cruz",
          "email_address": "juan_cruz@email.com",
          "wallet_address": "9ZNTfG4NyQgxy2SWjSiQoUyBPEvXT2xo7fKc5hPYYJ7b"
        }
      ]
    }
    ```

- When the user decides to transfer the e-Certificate to its recipient(s), the system should make an HTTP POST request to the `/issuances` endpoint.

    The HTTP POST request body should be the JSON object returned by the `/certificates` endpoint.

    The `/issuances` endpoint should return a JSON object containing information about the transfer.

    Example response body:

    ```json
    {
      "transfer_id": "9b5ec38f-4d0f-4a8b-9a36-60feba6ea7b1",
      "certificate_template_id": "900bb16f-77a8-47d0-82a4-5a7b10b34296",
      "certificate_config_id": "cbddb991-41f3-44b7-a377-f5d98755e881",
      "issue_date": "2022-01-31",
      "recipients": [
        {
          "recipient_name": "Juan Cruz",
          "email_address": "juan_cruz@email.com",
          "wallet_address": "9ZNTfG4NyQgxy2SWjSiQoUyBPEvXT2xo7fKc5hPYYJ7b",
          "certificate_id": "8561b082-bcdd-4ff0-9506-ee1dcebda715"
        }
      ]
    }
    ```

### **Verification**

The user should be able to verify the legitimacy of their e-Certificate by following the process described in this section.

- Each e-Certificate is issued with a unique ID (UUID). Users can verify this ID by making an HTTP GET request to the `/certificates` endpoint and attaching said ID as a path parameter, like so:

```
/certificates/8561b082-bcdd-4ff0-9506-ee1dcebda715
```

If the e-Certificate is legitimate, the `/certificates` endpoint should return truish value, otherwise it should return a falsy value.

Example response body:

```json
{
  "verified": true
}
```

> Tip: A friendly user interface should represent this endpoint.
