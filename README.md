# OAuth Proxy

Authentication and authorization middleware using OAuth2 and OIDC protocols. It protects web applications and APIs by acting as a reverse proxy that enforces identity-based access control.

## What this is

OAuth Proxy is a lightweight server that handles OAuth2 and OpenID Connect (OIDC) authentication and verification on behalf of downstream services. Instead of integrating OAuth handling into every application, services can delegate authentication to this proxy. The proxy validates identity tokens and only forwards requests to protected backends for authenticated and authorized users.

## What this repository contains

- **`server`** — A Bottle-based Python server implementing the OAuth2/OIDC proxy logic.
- **`examples/`** — Sample integrations in Crystal and Python showing how to use the proxy in an application.
- **Key verification** — Ed25519 signature verification using a configured private key.

## Running the server

Install the requirements:

```bash
pip3 install -r requirements.txt
```

Make sure to have the secret in `/opt/key.priv` encoded using Base64. An example using `PyNaCl`:

```python
k = nacl.signing.SigningKey.generate()  # Usually the key is generated using a seed
k.encode(encoder=nacl.encoding.Base64Encoder).decode()  # What should be in the file
```

The application is a simple Bottle server, and it can be run using `uwsgi` as follows:

```bash
uwsgi --http :{port number} -w server
```

## Using the OAuth server in your application

In order to use this server, the application server must implement the following two endpoints:

- An endpoint that gets the public key from the OAuth server and redirects the user to the identity provider's correct URL.
- A callback endpoint that receives the data from the identity provider and sends it to the OAuth server to verify.

You can find a Crystal and Python example [here](examples/README.md).

## Role in the stack

OAuth Proxy sits at the edge of services that require authenticated access. It integrates with identity providers to secure downstream resources, offloading authentication complexity from application code. It is suitable for protecting admin panels, APIs, and internal tools.

## Relation to ThreeFold

This technology is used within the ThreeFold ecosystem and was first deployed on the ThreeFold Grid. It integrates with ThreeFold identity providers to secure resources within that ecosystem. The component itself is designed as reusable infrastructure technology and should be understood by its technical function first, independent of any specific deployment.

## Ownership

This repository is owned and maintained by TF-Tech NV, a Belgian company responsible for the development and maintenance of this technology.

## License

This project is licensed under the Apache License 2.0 — see the [LICENSE](LICENSE) file for details.
Copyright (c) TF-Tech NV.
