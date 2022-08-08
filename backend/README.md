# REST API for Certinize's Back End

## Table of Contents

- [Get Started](#get-started)
    - [Authorization](#authorization)
- [Tasks](#tasks)
    - <details>
        <summary><a href="#certificates">Certificates</a></summary>
        <br>
        <ul>
            <li><a href="#generate-ecertificate">Generate eCertificate(s)</a></li>
        </ul>
    </details>

    - <details>
        <summary><a href="#configurations">Configurations</a></summary>
        <br>
        <ul>
            <li><a href="#create-template-configuration">Create Template Configuration</a></li>
            <li><a href="#get-template-configuration">Get Template Configuration</a></li>
            <li><a href="#list-template-configurations">List Template Configuration</a></li>
        </ul>
    </details>

    - <details>
        <summary><a href="#issuances">Issuances</a></summary>
        <br>
        <ul>
            <li><a href="#transfer-certificate">Transfer Certificate</a></li>
        </ul>
    </details>

    - <details>
        <summary><a href="#templates">Templates</a></summary>
        <br>
        <ul>
            <li><a href="#list-templates">List Templates</a></li>
        </ul>
    </details>

    - <details>
        <summary><a href="#users">Users</a></summary>
        <br>
        <ul>
            <li><a href="#auth-solana-user">Auth Solana User</a></li>
        </ul>
    </details>


# Get Started

## Authorization

Authorization is a one-step process. You should be able to access most endpoints by attaching your wallet address to the `/users` endpoint as a path parameter and making an HTTP GET request.

Example request:
```
/users/9ZNTfG4NyQgxy2SWjSiQoUyBPEvXT2xo7fKc5hPYYJ7b
```

The server should then return an associated API key (UUID).

Example response:

```json
{
  "wallet_address": "9ZNTfG4NyQgxy2SWjSiQoUyBPEvXT2xo7fKc5hPYYJ7b",
  "api_key": "e8e5725b-7f33-5bad-9438-e63bdbd2efea",
  "user_id": "5ca167d6-1654-11ed-908f-00155d3ecff4"
}
```

# Tasks

## Certificates

### Generate eCertificate

    - Method: POST
    - Path: /certificates
    - Description: Generate an e-Certificate.

**Request**

Supported Media Types:

- `application/json`

**Response**

Supported Media Types:

- `application/json`

**Response Status Codes**

- 201 Response: The e-Certificate(s) was created.
- 400 Response: Bad request syntax or unsuported method.

**Examples**

Request Header:

```
X-API-Key: 0ef3c43d-a3ac-52a1-938b-57cff7e60282
```

Request Body:

```json
{
  "template_config_id": "36b91a30-16fb-11ed-8a9e-00155d3ecff4",
  "recipients": [
    {
      "recipient_name": "Juan Cruz",
      "email_address": "juan_cruz@email.com",
      "wallet_address": "9ZNTfG4NyQgxy2SWjSiQoUyBPEvXT2xo7fKc5hPYYJ7b"
    }
  ]
}
```

HTTP Status Code:

```
201
```

JSON Response:

```json
{
  "certificate_collection_id": "6a5cfab5-6451-4515-abb3-c6bd4033e21e",
  "template_id": "e82fbcd5-3f45-483e-887b-5ffec0194c42",
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
      "wallet_address": "9ZNTfG4NyQgxy2SWjSiQoUyBPEvXT2xo7fKc5hPYYJ7b",
      "generated_certificate": "https://bucket.s3.objectstorageprovider.com/generated_certificate.jpg"
    }
  ]
}
```

## Configurations

### Create Template Configuration

    - Method: POST
    - Path: /configurations
    - Description: Save an e-Certificate template configuration.

**Request**

Supported Media Types:

- `application/json`

**Response**

Supported Media Types:

- `application/json`

**Response Status Codes**

- 201 Response: The e-Certificate template configuration was created.
- 400 Response: Bad request syntax or unsuported method.

**Examples**

Request Header:

```
X-API-Key: 0ef3c43d-a3ac-52a1-938b-57cff7e60282
```

Request Body:

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
  "template_id": "e82fbcd5-3f45-483e-887b-5ffec0194c42",
  "template_config_name": "example"
}
```

HTTP Status Code:

```
201
```

JSON Response:

```json
{
  "template_config_id": "36b91a30-16fb-11ed-8a9e-00155d3ecff4",
  "template_config_name": "example",
  "config_meta": {
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
    "template_id": "e82fbcd5-3f45-483e-887b-5ffec0194c42"
  }
}
```

### Get Template Configuration

    - Method: GET
    - Path: /configurations/{template_config_id:uuid}
    - Description: Get a specific template configuration.

**Request**

Path Parameters:

```
template_config_id:uuid
```

**Response**

Supported Media Types:

- `application/json`

**Response Status Codes**

- 200 Response: The request was fulfilled.
- 400 Response: Bad request syntax or unsuported method.

**Examples**

Request Header:

```
X-API-Key: 0ef3c43d-a3ac-52a1-938b-57cff7e60282
```

Request Parameter:

```
/configurations/36b91a30-16fb-11ed-8a9e-00155d3ecff4
```

HTTP Status Code:

```
200
```

JSON Response:

```json
{
  "template_config_id": "36b91a30-16fb-11ed-8a9e-00155d3ecff4",
  "template_config_name": "example",
  "config_meta": {
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
    "template_id": "e82fbcd5-3f45-483e-887b-5ffec0194c42"
  }
}
```

### List Template Configurations

    - Method: GET
    - Path: /configurations
    - Description: Get a list of template configurations.

**Request**

There are no request parameters for this operation.

**Response**

Supported Media Types:

- `application/json`

**Response Status Codes**

- 200 Response: The request was fulfilled.
- 400 Response: Bad request syntax or unsuported method.

**Examples**

Request Header:

```
X-API-Key: 0ef3c43d-a3ac-52a1-938b-57cff7e60282
```

Request Path:

```
/configurations
```

HTTP Status Code:

```
200
```

JSON Response:

```json
{
  "configurations": [
    {
      "template_config_id": "36b91a30-16fb-11ed-8a9e-00155d3ecff4",
      "template_config_name": "example",
      "config_meta": {
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
        "template_id": "e82fbcd5-3f45-483e-887b-5ffec0194c42"
      }
    },
    ...
  ]
}
```

## Issuances

### Transfer Certificate

    - Method: POST
    - Path: /issuances
    - Description: Transfer an e-Certificate to a recipient(s).

**Request**

Path Parameters:

```
certificate_collection_id:uuid
```

**Response**

Supported Media Types:

- `application/json`

**Response Status Codes**

- 201 Response: The transfer request was fulfilled.
- 400 Response: Bad request syntax or unsuported method.

**Examples**

Request Header:

```
X-API-Key: 0ef3c43d-a3ac-52a1-938b-57cff7e60282
```

Request Parameter:

```
/issuances/34f8ba1a-1707-11ed-a111-f73a0f2a24a8
```

HTTP Status Code:

```
201
```

JSON Response:

```json
{
  "transfer_id": "9b5ec38f-4d0f-4a8b-9a36-60feba6ea7b1",
  "template_id": "e82fbcd5-3f45-483e-887b-5ffec0194c42",
  "template_config_id": "36b91a30-16fb-11ed-8a9e-00155d3ecff4",
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

## Templates

### List Templates

    - Method: GET
    - Path: /templates
    - Description: Get a list of e-Certificate templates.

**Request**

There are no request parameters for this operation.

Supported Media Types:

- `application/json`

**Response Status Codes**

- 200 Response: The request was fulfilled.
- 400 Response: Bad request syntax or unsuported method.

**Examples**

Request Header:

```
X-API-Key: 0ef3c43d-a3ac-52a1-938b-57cff7e60282
```

Request Body:

```json
{
  "template_id": "e82fbcd5-3f45-483e-887b-5ffec0194c42",
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

HTTP Status Code:

```
200
```

JSON Response:

```json
{
  "transfer_id": "9b5ec38f-4d0f-4a8b-9a36-60feba6ea7b1",
  "template_id": "e82fbcd5-3f45-483e-887b-5ffec0194c42",
  "template_config_id": "36b91a30-16fb-11ed-8a9e-00155d3ecff4",
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

## Users

### Auth Solana User

    - Method: POST
    - Path: /users
    - Description: Transfer an e-Certificate to a recipient(s).

**Request**

Path Parameters:

```
wallet_address:str
```

**Response**

Supported Media Types:

- `application/json`

**Response Status Codes**

- 200 Response: The user was successfully authorized.
- 400 Response: Bad request syntax or unsuported method.

**Examples**

Request Header:

```
X-API-Key: 0ef3c43d-a3ac-52a1-938b-57cff7e60282
```

Request Parameter:

```
/users/9ZNTfG4NyQgxy2SWjSiQoUyBPEvXT2xo7fKc5hPYYJ7b
```

HTTP Status Code:

```
200
```

JSON Response:

```json
{
  "wallet_address": "9ZNTfG4NyQgxy2SWjSiQoUyBPEvXT2xo7fKc5hPYYJ7b",
  "api_key": "e8e5725b-7f33-5bad-9438-e63bdbd2efea",
  "user_id": "5ca167d6-1654-11ed-908f-00155d3ecff4"
}
```

## Database

![db-design](db-design.svg)
