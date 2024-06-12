---
# Banner
banner:
  title: "L402: The Missing Piece in the Internet's Payment Infrastructure"
  content: "L402 is an open protocol that implements internet-native paywalls by building upon the HTTP 402 Payment Required status code and the Lightning Network."

  button:
    enable: true
    label: "Live demo"
    link: "https://colab.research.google.com/drive/1MLZy1g6-lFqbRAfFOxR14PZ3b36sYr1r"
    
# Diagram

# Features
feature_list:
  - title: New Monetization Models
    description: Unlocks new revenue streams by enabling micropayments, pay-per-use and granular access control models.

  - title: Open and Extensible
    description: L402's open protocol encourages innovation and wide adoption across industries, fostering a thriving ecosystem of applications and services.
  
  - title: Language agnostic
    description: L402's design is universally compatible, ensuring seamless integration across various programming environments and platforms.

  - title: Native Digital Payments
    description: Integrates with the Lightning Network for instant, low-cost transactions, perfect for API monetization and digital services.

  - title: Granular Access Control
    description: Leveraging macaroons, L402 enables fine-grained access control and secure token management for enhanced security and flexibility.

  - title: AI-Friendly Protocol
    description: Built on HTTP, the Lightning Network, and Macaroons, L402 provides a machine-friendly scheme perfect for AI applications and automated systems.

# Implementation Details
implementation_details:
  - title: "Server: Challenge Details"
    content: |
      Before processing a request to a protected endpoint, the server issues an L402 challenge by returning a 402 Payment Required HTTP response with the challenge details encoded in a response header. 
      
      The challenge includes a macaroon (a token granting specific permissions) and a Lightning Network invoice.
    
    code: |
      macaroon = "MDAzNmxvY2F0aW9uIGh0dHBzOi8vbHNhdC1wbGF5Z3Jv..."
      invoice = "lnbc12340n1pny96vwpp5hc4l9wl8ze9jajratkzuxa5..."

      challenge = "L402 macaroon="{macaroon}"  invoice="{invoice}"

      # The challenge is returned in the response headers 
      response.headers['WWW-Authenticate'] = challenge

  - title: "Client: Authentication Header"
    content: |
      Upon receiving an HTTP 402 Payment Required status, the client extracts the macaroon and Lightning invoice from the challenge. 
      
      The client then pays the Lightning invoice and obtains the preimage. Using the macaroon and preimage, the client constructs an authenticated request to the server, which will now be processed since the client has completed the required payment and authentication steps.

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
    content: |
      Macaroons are a crucial component of the L402 protocol, providing a secure and flexible way to manage client permissions.
      
      They are bearer tokens that contain caveats, which specify the constraints and limitations of the token. In the context of L402, macaroons are used to grant access to specific resources or services based on the client's payment. 

      The server generates a macaroon with the appropriate caveats, such as an expiration time or scope limitations, and includes it in the challenge header.

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
