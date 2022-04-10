# Documentation for REST API endpoints

To access the API endpoints, first obtain an API key. For all subsequent requests, include the API key in the header. 

```
Authorization: Bearer <API key>
```

### Register as an issuer

To be able to issue verifiable credential, send a `POST` request to `https://onboard-id.herokuapp.com/api/registerIssuer` with body of the following format:
```
{
    issuerName: <issuer name>
}
``` 

If successful, server response as follows:
```
{
    issuerDID: <unique identifier of the credential issuer>
}
```

### Specify credential type and attributes

To create a verifiable credential, send a `POST` request to `https://onboard-id.herokuapp.com/api/createCredential` with body of the following format: 
```
{
    name: "Credential Name",
    version: "1.0", 
    attributes: attributes, // an array which contains attributes to be included in credential
}
```
An example attributes array is::
```
attributes = ['Name','Passport Number','KYC Process Date']
```

If successful, server response as follows: 
```
{
    definitionId: <a unique identifier for the created credential type>
}
```

### Issuing credential to end user

To issue a verifiable credential to the end user which defined attribute values, send a `POST` request to `https://onboard-id.herokuapp.com/api/issueCredential` with body of the following format: 
```
{
    definitionId: <unique identifier for credential type>,
    credentialValues: <JSON body for credential attribute values>
}
```

Assuming the credential type has the example attributes described above, then an example credentialValues JSON as follows:
```
credentialValues = {
    "Name": "Satoshi Nakamoto",
    "Passport Number": "A12345678",
    "KYC Process Date": "2022-01-01"
}
```

If successful, server response as follows: 
```
{
    invitation: <a unique URL that allows end users to receive the credential is their mobile wallet>,
    credentialId: <unique identifier for that specific credential>
}
```

### Revoke credential

If the credential issuer later wants to revoke a previously-issued credential, send a `POST` request to `https://onboard-id.herokuapp.com/api/revokeCredential` with body of the following format: 
```
{
    definitionId: <unique identifier for credential type>,
    credentialId: <unique identifier for that specific credential>
}
```

If successful, server response is a `200` code. 

### Verify credential 

To verify a credential presented by an end user, send a `POST` request to `https://onboard-id.herokuapp.com/api/verifyCredential` with body of the following format:
```
{
    definitionId: <unique identifier for credential type>
}
```

If successful, server response as follows:
```
{
    verificationRequestUrl: <a unique URL that allows end users to prove their credentials>,
    verificationId: <unique identifier for verification request>
}
```

### Check credential verification

To check whether the credential presented by an end user is valid, send a `POST` request to `https://onboard-id.herokuapp.com/api/checkVerification` with body of the following format:
```
{
    verificationId: <unique identifier for verification request>
}
```

If the credential presented by the end user is a valid credential, server response as follows:
```
{
    verification: <a JSON containing the correctness and attributes value of the credential>
}
```

For example, if the credential presented is the example credential mentioned earlier, an example response as follows:
```
{
    verification: {
        credentialValues: {
            "Name": "Satoshi Nakamoto",
            "Passport Number": "A12345678",
            "KYC Process Date": "2022-01-01"
        },
        verificationValidity: true,
        issuerDID: <unique identifier of the credential issuer>
    }
}
```
