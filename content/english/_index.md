---
# Banner
banner:
  title: "L402: Internet-Native Paywalls"
  content: "L402 is a decentralized payment protocol built upon the Lightning Network. It utilizes a challenge-response mechanism where a server issues a challenge in the form of a 402 error code and a custom header containing a macaroon and an invoice. The client then pays the invoice and retrieves a preimage, which is used to complete the authentication process."
  image: "/images/l402-protocol-sequence-diagram.png"
  button:
    enable: false
    label: "Get Started For Free"
    link: "https://github.com/zeon-studio/hugoplate"



# Implementation Details
implementation_details:
  - title: "Server Challenge Details"
    content: "This section outlines the server challenge mechanism used for authentication and authorization. The server sends a challenge header with specific tokens required for further interactions."
    code: |
      macaroon = "MDAzNmxvY2F0aW9uIGh0dHBzOi8vbHNhdC1wbGF5Z3Jv..."
      invoice = "lnbc12340n1pny96vwpp5hc4l9wl8ze9jajratkzuxa5..."

      challenge = "L402 macaroon="{macaroon}"  invoice="{invoice}"
      
      # The challenge is returned in the response headers 
      response.headers['WWW-Authenticate'] = challenge

  - title: "L402 Authentication Header Details"
    content: "This section provides details on the L402 authentication header used for secure transactions. The header includes a macaroon and a preimage for enhanced security."
    code: |
      # Client prepares the authentication header with the 
      # macaroon and preimage
      challenge = response.headers['WWW-Authenticate']
      macaroon, invoice = parse_challenge(challenge)
      
      # Pay the lightning invoice to receive the preimage
      preimage = pay_invoice(invoice)
      auth_header = f"L402 {macaroon}:{preimage}"

      # Client sends a request with the authentication header
      headers = {'Authorization': f"L402 {macaroon}:{preimage}"}
      requests.get(url, headers=headers)


  - title: "Macaroons"
    content: "Macaroons are a form of token-based authorization that allows for fine-grained permissions and constraints. They are used extensively in decentralized systems for secure and flexible user authentication."
    code: |
      # URL where the credentials can be used
      location = "fewsats.com"
      version = 0
      payment_hash = "35cf3da4dfdefa01a3859659d447eb2eeb070c9c6610f4..."
      token_id = "7133548b39c094b83120052b10685d7cc5..."

      # The identifier needs to be encoded as bytes
      identifier = encode_id(version, payment_hash, token_id)

      # The caveats can be used to add constraints to the macaroon
      caveats = [
        "expires_at=2024-06-14T01:38:46Z"
      ]

      macaroon = create_macaroon(identifier, location, caveats)

      print(macaroon.encode())
      # AgELZmV3c2F0cy5jb20CQgAANc89pN/e+gG...

      print(macaroon.json())
      # {
      #   "ID": "000035cf3da4dfdefa01a385965...",
      #   "version": 0,
      #   "payment_hash": "35cf3da4dfdefa01a385965...",
      #   "token_id": "7133548b39c094b83120052b106...",
      #   "location": "fewsats.com",
      #   "caveats": [
      #     "expires_at=2024-06-14T01:38:46Z"
      #   ]
      # }

# Features
# features:
#   - title: "Key Components"
#     image: "/images/service-1.png"
#     content: "Key components of the L402 protocol include:"
#     bulletpoints:
#       - "402 error code: This HTTP status code signifies 'Payment Required' and is used as a trigger for the L402 protocol."
#       - "Macaroon: As described in Google's 2014 paper on 'Decentralized Authorization,' is a bearer token that holds caveats (constraints) defining access permissions."
#       - "Invoice: In the context of L402, the invoice is typically a Lightning Network invoice, although other payment methods can be integrated through external services."
#       - "Preimage: The preimage is a secret value associated with the invoice. It is revealed to the client upon payment and is used to complete the authentication challenge."
#     button:
#       enable: false
#       label: "Get Started Now"
#       link: "#"

#   - title: "Discover the Key Features Of Hugo"
#     image: "/images/service-2.png"
#     content: "Hugo is an all-in-one web framework for building fast, content-focused websites. It offers a range of exciting features for developers and website creators. Some of the key features are:"
#     bulletpoints:
#       - "Zero JS, by default: No JavaScript runtime overhead to slow you down."
#       - "Customizable: Tailwind, MDX, and 100+ other integrations to choose from."
#       - "UI-agnostic: Supports React, Preact, Svelte, Vue, Solid, Lit and more."
#     button:
#       enable: true
#       label: "Get Started Now"
#       link: "https://github.com/zeon-studio/hugoplate"

#   - title: "The Top Reasons to Choose Hugo for Your Hugo Project"
#     image: "/images/service-3.png"
#     content: "With Hugo, you can build modern and content-focused websites without sacrificing performance or ease of use."
#     bulletpoints:
#       - "Instantly load static sites for better user experience and SEO."
#       - "Intuitive syntax and support for popular frameworks make learning and using Hugo a breeze."
#       - "Use any front-end library or framework, or build custom components, for any project size."
#       - "Built on cutting-edge technology to keep your projects up-to-date with the latest web standards."
#     button:
#       enable: false
#       label: ""
#       link: ""
---
