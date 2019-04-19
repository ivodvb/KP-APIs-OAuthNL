Additional specification and constraints of iGov-NL to the iGov profile
=====

# Introduction
This document is a companion to the NL GOV Assurance profile for OAuth 2.0(iGov-NL). It lists all Added specification and additional constraints of iGov-NL. It provides a compact overview of the differences between the international and dutch profile. For each difference it points to a rationale.

## 1.3.3.1 Requests to the Authorization Endpoint
iGov-NL

Native clients MUST apply PKCE, as per RFC7636. As code_verifier the S256 method MUST be applied. Effectively this means that a Native Client MUST include a cryptographic random code_challenge of at least 128 bits of entropy and the code_challenge_method with the value S256.

Request fields:

client_id
Mandatory. MUST have the value as obtained during registration.
scope
Optional.
response_type
Mandatory. MUST have value `code` for the Authorization Code Flow.
redirect_uri
Mandatory. MUST be an absolute HTTPS URL, pre-registered with the Authorization Server.
state
Mandatory, see above. Do not use the SessionID secure cookie for this.
code_challenge
In case of using a native app as user-agent mandatory. (Eg. an UUID [rfc4122])
code_challenge_method
In case `code_challenge` is used with a native app, mandatory. MUST use the value `S256`.
/iGov-NL

**rationale to be provided by: Remco Schaar**

**reference to rationale:**

## 1.3.3.2 Response from the Authorization Endpoint
iGov-NL

Response parameters

code
Mandatory. MUST be a cryptographic random value, using an unpredictable value with at least 128 bits of entropy.
state
Mandatory. MUST be a verbatim copy of the value of the state parameter in the Authorization Request.
/iGov-NL

**rationale to be provided by: Remco**

**reference to rationale:**

## 1.3.3.3 Requests to the Token Endpoint
iGov-NL

In addition to above signing methods, the Authorization server SHOULD support PS256 signing algorithm [RFC7518] for the signing of the private_key_jwt.

Effectively, the Token Request has the following content:

grant_type
Mandatory. MUST contain the value `authorization_code`
code
Mandatory. MUST be the value obtained from the Authorization Response.
scope
Optional. MUST be less or same as the requested scope.
redirect_uri
Mandatory. MUST be an absolute HTTPS URL, pre-registered with the Authorization Server.
client_id
Mandatory. MUST have the value as obtained during registration.
client_assertion_type
Mandatory. MUST have the value `urn:ietf:params:oauth:client-assertion-type:jwt-bearer`, properly encoded.
client_assertion
Mandatory. MUST have the above specified signed JWT as contents.
/iGov-NL

**rationale to be provided by:**

**reference to rationale:**

## 1.3.3.4 Client Keys

iGov-NL

In case the Authorization Server, Resource Server and client are not operated under responsibility of the same organisation, each party MUST use PKIoverheid certificates with OIN.

The PKIoverheid certificate MUST be included as a x5c parameter. The x5c parameter MUST be included as a list (array) of X509 certificate(s), as Base64 DER encoded PKIoverheid certificate(s). The first certificate MUST be the Client's certificate, optionally followed by the rest of that certificate's chain. The jwks structure MUST include the public key parameters with the same values of the corresponding X509 certificate included as x5c, as per [rfc7517] §4.7.

/iGov-NL

**rationale to be provided by: Frank**

**reference to rationale: Detailed rationale 1**

## 1.3.3.5 Token Response

iGov-NL TODO logischer om "Token Response" in AS profile te beschrijven ipv in client profile.

The Token Response has the following contents:

access_token
Mandatory. Structured access token a.k.a. a JWT Bearer token. The JWT MUST be signed.
token_type
Mandatory. The type for a JWT Bearer token is Bearer, as per [rfc6750]
refresh_token
Under this profile, refresh tokens are (currently) not supported and MUST NOT be used.
expires_in
Optional. Lifetime of the access token, in seconds.
scope
Optional. Scope(s) of the access (token) granted, multiple scopes are separated by whitespace. The scope MAY be omitted if it is identical to the scope requested.
For best practices on token lifetime see section Token Lifetimes. /iGov-NL

**rationale to be provided by: Remco**

**reference to rationale:**

## 1.4.1.3 Dynamic Registration
iGov-NL

In this version of iGov-NL we follow iGov for the requirement that the Authorization servers MUST support dynamic client registration. However depending on how the future authentication architecture of the dutch government develops in regards to OAuth we may revisit this in a future revision. The current requirement fits an architecture where there is a limited number of widely used authorization servers. However if in practice we start seeing a very large number of authorization servers with limited use this requirement can become a reccomendation in a future version of this profile. For these authorization servers with limited use we consider mandatory support for dynamic client registration a large burden.

/iGov-NL

**rationale to be provided by:**

**reference to rationale:**

## 1.4.1.5 Discovery
iGov-NL

iGov requires that the authorization server provides an OpenIDConnect service discovery endpoint. Recently OAuth 2.0 Authorization Server Metadata [rfc8414] has been finalized, this provide the same functionality in a more generic way and could replace this requirement in a future version of the iGov-NL profile.

/iGov-NL

**rationale to be provided by:**

**reference to rationale:**

## 1.4.2.1 JWT Bearer Tokens

iGov-NL

In iGov-NL the sub claim MUST be present.

/iGov-NL

**rationale to be provided by:**

**reference to rationale:**

iGov-NL

In addition to above signing methods, the Authorization server SHOULD support PS256 signing algorithm [RFC7518] for the signing of the JWT Bearer Tokens.

/iGov-NL

**rationale to be provided by:**

**reference to rationale:**

iGov-NL

How to select or obtain the key to be used for encryption of an access token is out of scope of this profile. A early draft of "Resource Indicators for OAuth 2.0" exist and could be used. This draft describes usage of the resource parameter to indicate the applicable resource server.

**reference to rationale: self explanatory: additional information (non normative) on how to implement requirement**

In case the Authorization Server, Resource Server and client are not operated under responsibility of the same organisation, each party MUST use PKIoverheid certificates with OIN for encryption.

**rationale to be provided by: Frank**

**reference to rationale: Detailed rationale 1**

/iGov-NL


## 1.5.1 Protecting Resources

iGov-NL

TODO NL example

/iGov-NL

**rationale to be provided by: Jan-Jaap**

**reference to rationale: self explanatory: additional information (non normative) on how to implement requirement**

## 1.5.2 Connections with Clients

iGov-NL

A Protected Resource under this profile MUST NOT accept access tokens passed using the query parameter method.

A Protected Resource under this profile SHOULD if verify if the client is the Authorized party (AZP) when client authentications is used. See section Advanced OAuth Security Options as well.

/iGov-NL

**rationale to be provided by:**

**reference to rationale:**

## 1.6.1 Proof of Possession Tokens

iGov-NL

Proof of possession can be implemented using various methods. An example of such an implementation is using TLS with mutual authentication, where the client is using a PKIoverheid certificate. The authorized party (azp) can then be verified with the client certificate to match the authorized party. As an alternative, the authorization server can include a cnf parameter in the JWT by the authorization server, see [rfc7800]. The key referenced in cnf can be validated using a form of client authentication, e.g. using an private_key_jwt.

/iGov-NL

**rationale to be provided by:**

**reference to rationale: self explanatory: additional information (non normative) on how to implement requirement**

## 1.7 Security Considerations

iGov-NL

In addition to the Best Current Practice for TLS, it is highly RECOMMENDED for all conforming implementations to incorporate the TLS guidelines from the Dutch NCSC into their implementations. If these guidelines are applied:

For back-channel communication, the guidelines categorized as "good" MUST be applied.
For front-channel communication, the guidelines for "good" MUST be applied and the guidelines for "sufficient" MAY be applied, depending target audience and support requirements.
Guidelines categorized as "insufficient" MUST NOT be applied and those categorized as "deprecated" SHOULD NOT be used.
/iGov-NL

**rationale to be provided by: Martin**

**reference to rationale: Detailed rationale 2 **

# General Rationale

**TODO translate to english**

The reason for the creation of iGov-NL is an advisory document on the adoption of OAuth as a mandatory(comply or explain) standard for the Dutch public sector. It states that a Dutch profile is needed for the OAuth standard to avoid interoperability problems between different implementations. The existance of iGov-NL has become a precondition for the adaoption of OAuth as mandatory standard for the Dutch public sector.
 
## Expert advies OAuth forum standaardisatie
Het opstellen van deze standaard is voortgekomen uit het Expert advies OAuth [Expert]. Daarin wordt aangeraden eerst een nederlands profiel op stellen alvorens OAuth op de pas toe of leg uit lijst van het forum standaardisatie te plaatsen.

## Werkingsgebied standaard
Als organisatorisch werkingsgebied wordt geadviseerd: Nederlandse overheden (Rijk, provincies, gemeenten en waterschappen) en instellingen uit de (semi-) publieke sector

## Toepassingsgebied standaard
Als functioneel toepassingsgebied wordt voorgesteld: Het gebruik van OAuth 2.0 is verplicht voor applicaties waarbij gebruikers (resource owner) toestemming geven (impliciet of expliciet) aan een dienst (van een derde) om namens hem toegang te krijgen tot specifieke gegevens via een RESTful API. Het gaat dan om een RESTful API waar de resource owner recht tot toegang heeft.

## OpenID connect buiten scope
de expertgroep is op 7 juli en op 22 september 2016 bijeengekomen om de standaarden, de aandachtspunten en openstaande vragen uit het voorbereidingsdossier te bespreken. Daarbij is vastgesteld dat OpenID Connect niet voor opneming op de lijst open standaarden in aanmerking komt.

## Aansluiting op internationale standaard iGov
Het Nederlands profiel OAuth baseren we het internationale iGOV OAuth 2.0 profiel [iGOV.OAuth2] we nemen niet alle keuzes van dit internationale profiel over aangezien dit een aantal keuzes bevat die sterk leunen op de amerikaanse situatie. Het kan het best beschouwd worden als een fork waar we in ons profiel aangeven waar we afwijken. iGov heeft twee naast het OAuth profiel ook een OpenID connect profiel [iGOV.OpenID] wanneer mogelijk ook OpenID connect op de pas toe of leg uit lijst van het Forum standaardisatie komt kan dit Nederlandse profiel uitgebreid worden met een Nederlandse variant van het iGov OpenID Connect profiel. De usecase die hieronder wordt beschreven sorteerd daar al op voor.

Het Nederlands profiel OAuth is hier te vinden: https://geonovum.github.io/KP-APIs-OAuthNL/#dutch-government-assurance-profile-for-oauth-2-0

# Detailed Rationale

## 1 Use of local standards for PKI certificates (PKIOverheid)

## 2 Use of local standards and best practices for TLS

## 3 Support of limited use case 


