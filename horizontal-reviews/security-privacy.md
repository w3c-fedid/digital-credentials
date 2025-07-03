# [Self-Review Questionnaire: Security and Privacy](https://w3ctag.github.io/security-questionnaire/)

> 01.  What information does this feature expose,
>      and for what purposes?

Potentially high-value, privacy-sensitive claims from digital credentials, such as both unique identification (such as for access to government services) and selective disclosures (such as for age verification) from digital driver's licenses. These requests are made from websites, to digital wallet applications, and back to websites, through user-mediated, one-time communication channels. These are designed to be better options than established lower-level communication channels like custom schemes, QR codes, and server-to-server network communications. A website can decide which information to request. A request can be inspected by the user agent, and the specification recommends that the information being requested be disclosed to the user in the permission prompt. The wallet application decides which information is actually shared, based upon the request.

> 02.  Do features in your specification expose the minimum amount of information
>      necessary to implement the intended functionality?

A primary use case of the digital credentials API is to enable users to selectively disclose only the minimum required information, such as a cryptographic attestation that the user holds a California driver’s license for an adult. The use of selective disclosure, however, is a decision for the verifier website based on the use case. There are legitimate scenarios, such as creating or recovering an account on a government website, where uniquely identifiable and potentially non-resettable personally identifiable information (PII) might be exposed.

The API is designed to expect the use of response encryption so that this PII is exposed only to the requesting server, such that exposure to code running in the web page or browser can be minimized or even avoided. It is an [open question](https://github.com/WICG/digital-identities/issues/109) whether this response encryption is something we can reasonably enforce at this layer.

The API is also designed to require request transparency to enable user agents and operating systems to appropriately inform users about the level of privacy risk involved in the request. The specification has a set of recommendations ([1](https://www.w3.org/TR/digital-credentials/latest/#user-permission-and-transparency), [2](https://www.w3.org/TR/digital-credentials/latest/#mitigating-unnecessary-requests-for-government-credentials), [3](https://www.w3.org/TR/digital-credentials/latest/#mitigating-unnecessary-requests-for-non-government-credentials)) for implementers, describing how to appropriately inform users about the credentials being exchanged. For example, the [Chromium implementation](https://docs.google.com/document/d/1L68tmNXCQXucsCV8eS8CBd_F9FZ6TNwKNOaFkA8RfwI/edit) intends to show users stronger warnings in riskier scenarios such as those that do not support selective disclosure.

> 03.  Do the features in your specification expose personal information,
>      personally-identifiable information (PII), or information derived from
>      either?

Yes. The API is designed to help facilitate communication of personally-identifiable information (PII) from wallet applications to verifier web servers, through a relying party (a web application).

> 04.  How do the features in your specification deal with sensitive information?

The specification seeks to empower users, issuers, wallets and verifiers to present sensitive information in a more secure and private fashion than existing ad-hoc solutions.

Today, online identity verification (e.g., for know your customer (KYC)) is usually done by submitting photos of government identity documents. This is commonly done by users uploading photos containing the personally identifiable information (PII) using the web’s `<input type=file>` or similar mechanisms. Due to the privacy, security, and usability limits of this approach, credential issuers and verifiers are working to move to the use of wallet applications that can selectively disclose cryptographically-attested properties. Today, those approaches rely on generic browser-to-native-application communication paths, such as custom URL schemes and/or server-to-server communication.

The use of wallets aims to improve on the status quo of uploading images in a number of ways:
 * Enable selective disclosure (e.g., for more privacy-friendly age≥18 verification)
 * Enable the wallet to demonstrate legitimate possession of a credential by proving possession of key material bound to that credential
 * Enable end-to-end personally identifiable information (PII) encryption between the wallet application and the relevant verification server
 * Enable request authentication, where a wallet is required to cryptographically verify that the requester has permission to access the credential

Further, this specification aims to be an improvement over the existing communication channels websites use to talk to wallet apps by:
 * Enabling browsers and operating systems to provide meaningful credential selection and permission screens to users, prior to wallet applications becoming aware of the presentation request
 * Securely communicating the origin of the requesting site to the wallet application, so that it can implement its own MITM (man-in-the-middle) protection with support of WebPKI (Web Public Key Infrastructure)
 * Enabling browsers to inspect requests and provide additional UI affordances to users

> 05.  Do the features in your specification introduce state
>      that persists across browsing sessions?

Yes. The specification includes [a facility](https://w3c-fedid.github.io/digital-credentials/#create-origin-options-sameoriginwithancestors-internal-method) for issuing new credentials into wallets with user permission.

> 06.  Do the features in your specification expose information about the
>      underlying platform to origins?

Not directly. Wallets may (intentionally or inadvertently) expose some information through this API (such as some indication of which wallet app the user is using). But the primary motivation for these wallets over traditional federated authentication systems is that the wallet can act in the user’s interest to protect their privacy even from the credential issuer. Users will often have their choice of multiple wallets and are expected to use reputation for privacy as part of their decision making. Issuers may  choose which wallets to support their credentials in and can impose privacy requirements on those wallets. That said, there is absolutely a risk that users could be surprised or even deceived by some wallets exposing information they didn't expect. We are [exploring](https://github.com/WICG/digital-credentials/issues/117) ways in which our specification could potentially help reduce this risk.

> 07.  Does this specification allow an origin to send data to the underlying
>      platform?

Yes. The credential presentation request is sent to the underlying platform to mediate.

> 08.  Do features in this specification enable access to device sensors?

Not directly. See question 10.

> 09.  Do features in this specification enable new script execution/loading
>      mechanisms?

No.

> 10.  Do features in this specification allow an origin to access other devices?

Potentially, yes. While not a property of the DC API itself, the API is designed to support cross-device presentation flows such as by using the [FIDO CTAP](https://fidoalliance.org/specs/fido-v2.2-rd-20230321/fido-client-to-authenticator-protocol-v2.2-rd-20230321.html) protocol used by passkeys.

> 11.  Do features in this specification allow an origin some measure of control over
>      a user agent's native UI?

Not control, no. User agents may add additional affordances for user transparency and control.

> 12.  What temporary identifiers do the features in this specification create or
>      expose to the web?

None directly. Wallets may expose temporary identifiers, such as in the MSOs used for selective disclosure of ISO 18013-5 mdocs.

> 13.  How does this specification distinguish between behavior in first-party and
>      third-party contexts?

The specification uses a Permissions Policy with the "self" default allow list, meaning the API cannot be used in third-party contexts unless allowed by the (recursive) parent context(s).

> 14.  How do the features in this specification work in the context of a browser’s
>      Private Browsing or Incognito mode?

Like other browser authentication (e.g., WebAuthn) and identification (e.g., autofill) features, the feature is available to users to use in private browsing. Regardless of whether private browsing is in use, the feature is designed to not communicate information without the user’s explicit permission each and every time it's invoked. There are legitimate use cases for this API in private browsing such as privacy-preserving age verification.

> 15.  Does this specification have both "Security Considerations" and "Privacy
>      Considerations" sections?

The specification has [extensive Privacy considerations](https://w3c-fedid.github.io/digital-credentials/#privacy-considerations), most of which are still evolving as the group works through the many Privacy-related challenges of the digital credentials space.

There are [some early outlines for the Security Considerations](https://w3c-fedid.github.io/digital-credentials/#security-considerations), and there is active work in progress to write those up in more detail.

> 16.  Do features in your specification enable origins to downgrade default
>      security protections?

No

> 17.  What happens when a document that uses your feature is kept alive in BFCache
>      (instead of getting destroyed) after navigation, and potentially gets reused
>      on future navigations back to the document?

Implementations should fail or postpone any requests which are made while the page is not visible to the user. The spec [requires](https://www.w3.org/TR/digital-credentials/latest/#discoverfromexternalsource-origin-options-sameoriginwithancestors-internal-method) both user attention on a fully active document and user activation to use the API.

> 18.  What happens when a document that uses your feature gets disconnected?

See above. The spec has "fully active" checks in its algorithms.

> 19.  What should this questionnaire have asked?

What are the security and privacy implications of not shipping this feature? How does this feature fit into the larger privacy risk landscape? We believe this feature will offer reduced risks, relative to large-scale adoption of alternative technologies.

