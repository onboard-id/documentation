# Documentation for REST API endpoints

To access the API endpoints, first obtain an API key. For all subsequent requests, include the API key in the header. 

```
Authorization: Bearer <API key>
```

### Register as an issuer

To be able to issue verifiable credential, send a `POST` request to `https://onboardid.herokuapp.com/api/registerIssuer` with body of the following format:
```
{
    issuerName: <issuer name>
}
``` 

If successful, server response as follows:
```
{
    _id: <a unique identifier of the credential issuer>,
    APIkey: <a unique key to access the API endpoints>,
    issuerName: <issuer name provided>,
    tenantId: <a unique identifier of the credential issuer>,
    credentialDefinitionID: <an array which contains credential definition created by the issuer>
}
```

### Remove an issuer

To remove an issuer, send a `DELETE` request to `http://onboardid.herokuapp.com/api/removeIssuer` without any body.

If successful, server response as follows:
```
{
    MESSAGE: Successfully removed issuer with API key <API key provided>.
}
```

### Get issuer details

To get issuer details, send a `GET` request to `https://onboardid.herokuapp.com/api/getIssuerDetails` without any body.

If successful, server response as follows:
```
{
    _id: <a unique identifier of the credential issuer>,
    APIkey: <API key provided>,
    issuerName: <issuer name>,
    tenantId: <a unique identifier of the credential issuer>,
    credentialDefinitionID: <an array which contains credential definition created by the issuer>
}
```

### Specify credential type and attributes

To create a verifiable credential, send a `POST` request to `https://onboardid.herokuapp.com/api/createCredentialDefinition` with body of the following format: 
```
{
    name: "Credential Name",
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
    credentialDefinitionID: <a unique identifier for the created credential type>
}
```

### Issuing credential to end user

To issue a verifiable credential to the end user which defined attribute values, send a `POST` request to `https://onboardid.herokuapp.com/api/createCredential` with body of the following format: 
```
{
    alias: <credential alias>
    credentialDefinitionID: <a unique identifier for credential type>,
    credentialValues: <a JSON body for credential attribute values>
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
    invitation: <a unique URL that allows end users to receive the credential in their mobile wallet>,
    credentialID: <a unique identifier for that specific credential>
}
```

### Revoke credential

If the credential issuer later wants to revoke a previously-issued credential, send a `DELETE` request to `https://onboardid.herokuapp.com/api/revokeCredential` with body of the following format: 
```
{
    credentialDefinitionID: <a unique identifier for credential type>,
    credentialID: <a unique identifier for that specific credential>
}
```

If successful, server response as follows:
```
{
    MESSAGE: Successfully revoked credential with credential ID <credentialID provided>.
}
```

### Verify credential 

To verify a credential presented by an end user, send a `POST` request to `https://onboardid.herokuapp.com/api/verifyCredential` with body of the following format:
```
{
    credentialDefinitionID: <a unique identifier for credential type>
}
```

If successful, server response as follows:
```
{
    verificationRequestUrl: <a unique URL that allows end users to prove their credentials>,
    verificationID: <a unique identifier for verification request>
}
```

### Check credential verification

To check whether the credential presented by an end user is valid, send a `GET` request to `https://onboardid.herokuapp.com/api/checkVerification` with parameter:
```
    verificationID: <a unique identifier for verification request>
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

### Get Credential Definition Details
To get issuer details, send a `GET` request to `https://onboardid.herokuapp.com/api/getCredentialDefinitionDetails` with parameter:
```
    credentialDefinitionID: <a unique identifier for credential type>
```

If successful, server response as follows:
```
{
    _id: <a unique identifier of the credential definition>,
    credentialID: <an array which contains the credential ID created>,
    credentialDefinitionID: <Credential Definition ID provided>,
    verificationPolicyID: <a unique identifier for verification request>
    name: <credential definition name>,
    attributes: <an array which contains attributes to be included in credential>
}
```
