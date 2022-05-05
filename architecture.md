# System architecture

![alt text](./system-architecture.jpg?raw=true)

Our solution is exposed as backend REST API endpoints, where we abstract the underlying low-level technical details of interacting with the blockchain and smart contract. To integrate with our solution, issuers and verifiers simply need to call into our API endpoints. 

We are currently using Sovrin as the underlying ledger with the Indy DID method (`did:indy`). On our product roadmap, we are looking to migrate to the Cheqd network as the underlying ledger with the Cheqd DID method (`did:cheqd`), which is fully compliant with the [DID Core specification](https://www.w3.org/TR/did-core/) specified by the W3C. 
