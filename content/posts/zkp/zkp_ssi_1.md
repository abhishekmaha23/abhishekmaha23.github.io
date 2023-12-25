---
title: "ZKPs and SSI in practice in 2023 - Part 1"
subtitle: "A quick (and not comprehensive) guide on how to develop secure applications for the modern web!"
date: 2023-07-08
draft: false
layout: "post"
katex: true
---

# Part 1 - Self-sovereign identity

## Introduction

(To begin with, this is a multi-part post, that might have up to 3 or 4 parts. It's going to be long, and it's going to contain a few tangents to cool side-applications! But I'll try to keep things as relevant as possible.)

Privacy-consciousness is becoming mainstream, and regulations like the GDPR are actively advocating for the rights of citizens to be forgotten. However, we still have many many centralized systems run by large commercial organizations that invest incredible resources in storing things about users, using technology that would be brilliant from a technological perspective, if it wasn't passively contributing towards destabilizing civilization (through misdirected incentives). 

As individuals, we leave trails and patterns of usage across the internet everyday, and it is used to customize our experiences. When used for unethical purposes, this can be used to coerce us, maximize engagement and meet other such incentives that are not conducive to healthy and sustainable living.

A modern sequence of standards have picked up steam in the past decade to revolutionize the concept of the digital footprint, to counter centralized influences, and to bring the power of the web back to individuals. These are described below:

### Self-Sovereign Identities

Self-sovereign identity is a new paradigm towards Digital Identities. In the current form of the web, we create accounts with quite many service providers and governmental entities to gain access. The ability to have digital identifiers that are flexible, replaceable, digitally resolvable (also working offline if possible), chainable and delegatable is incredibly valuable! This concept is often proposed in two directions: (from these sources: [[1](https://www.cyberforge.com/can-less-be-more/)], [[2](https://trbouma.medium.com/less-identity-65f65d87f56b)])

- **LESS Identity** - Digital IDs provided by central authorities, governments, universities, etc. 
  The scope is reduced to be associated with certain credentials, identifiers. This identity should be a fraction of the digital presence of an individual, and ideally not reused to support association with multiple Verifiable credentials from multiple entities. 
- **Trust-less Identity** - Self-issued Digital IDs created through public-private key cryptography or published on the blockchain. These may be associated with service-providers, and can be replacable, short-lived and even dynamic.
  This allows individuals to create as many identifiers as needed for maximal privacy, even one-per-transaction if necessary.

In no case should people actually trust provide a single centralized ID to authenticate themselves.

| LESS Identity      | Trust-less Identity    |
| ------------------ | --------------------- |
| Minimum Disclosure | Anonymity             |
| Full Control       | Censorship Resistance | 
| Necessary Proofs   | Web of Trust          |
| Legally-Enabled    | Defend Human Rights vs. Powerful Actors                      |

Source : https://www.cyberforge.com/can-less-be-more/

It can be difficult to get a hold on the entire ecosystem of SSI in a single post, and the focus here is to look at how these concepts can be applied in a test use-case.

### Zero-Knowledge Proofs

Zero-Knowledge proofs (ZKP) were first introduced in a paper in 1985 [here](http://people.csail.mit.edu/silvio/Selected%20Scientific%20Papers/Proof%20Systems/The_Knowledge_Complexity_Of_Interactive_Proof_Systems.pdf), as an approach to minimize amount of knowledge communicated in proofs. It has since received significant interest as an approach to maximize privacy by generalizing the probabilistic approaches used within. ZKPs are extremely popular now due to the potential of applying them to trust-less systems like distributed ledgers, for which they are perfectly suited, but they also have major advantages in non-blockchain scenarios.

To give a brief introduction to the concept, it may be useful to refer to Vitalik Buterin's definitions [here](https://vitalik.ca/general/2021/01/26/snarks.html) and [here](https://vitalik.ca/general/2022/06/15/using_snarks.html) (the founder of Ethereum).

<div style="width: 50%;">

> Suppose that you have a public input \\(x\\), a private input \\(w\\), and a (public) function \\(f(x, w) \rightarrow \{True, False\}\\) that performs some kind of verification on the inputs. With a ZK-SNARK, you can prove that you know an \\(w\\) such that \\(f(x, w) = True\\) for some given \\(f\\) and \\(x\\), without revealing what \\(w\\) is. Additionally, the verifier can verify the proof much faster it would take for them to compute \\(f(x,w)\\) themselves, even if they know \\(w\\).

</div>

Essentially:
- A prover can prove a statement (secret) to a destination entity with the help of a verifier, without actually revealing the actual statement itself.
- This is done by providing a proof \\(\pi\\) which the verifier can validate very quickly, faster than the process of actually validating the entire secret.

A more in-depth definition and exploration of Zero-Knowledge techniques will be done in a future post.

### Problem statement

Let's define a couple of dummy problem statements to define what we want to achieve, and what this implies. These are trivial examples, and they may seem self-evident, but by implementing them, we get to see some of the less evident parts of the ecosystem.

**Case 1:**

Alice is a job applicant that needs to prove to Bob, a company recruiter that their credential, a 
graduate degree obtained from a university is valid, and that they received more than 90% in all classes they took through the degree.
In this slightly contrived example (somehow university degrees have become the standard example of verifiable credentials!), 
- The university doesn't need to know anything beyond what is necessary.
- The company doesn't want to know what courses Alice took - the job is completely unrelated to the degree, so the recruiter just wants to validate that Alice was academically outstanding.

A small diagram of all the stakeholders involved here would be as follows:
![Case 1 entities](../assets/zkp_ssi_1/Case1-basic.png)

**Case 2:**

Alice (the same person from the previous use-case) is a citizen who wants to take an international vacation to a country, and needs to buy a ticket from an airline. Airlines usually don't validate details at the time of purchasing tickets, but it would be quite useful to eliminate the immigration queue from airports - it's a complicated process that could just as well be done digitally.

So here, we have 4 stakeholders - Alice, the airline, the passport-issuing government and the visa-issuing government.

A combination of proofs from both governments is needed in collaboration to prove to the airline that they are legally able to go to the destination country. In this example,
- The airline wants to adhere to data privacy, and doesn't want to store more information than necessary for identifying an individual
- The governments shouldn't be able to identify Alice from verification requests made by the airline.

Visually represented, the entities here are:
![Case 2 entities](../assets/zkp_ssi_1/Case2-basic.png)

To achieve these within the current ecosystem of Self-Sovereign Identities and Zero-Knowledge Proofs, we may need to take it in steps. In this blog post, we'll explore Self-Sovereign Identities.

### Self-Sovereign Identity

Let's begin with a brief technology-agnostic introduction to SSI. We'll go briefly over the main questions, and in the end make a start on our implementation for our use-case.

Self-sovereign Identities are built on top of three pillars:
- *Decentralized Identifiers* - Digital Identifiers owned and managed by individuals, or the parties that they delegate trust to.
- *Verifiable Credentials* - Credentials denoting certifications, validations, real-world identity, and so on.
- *Verifiable Presentations* - Representations of Verifiable credentials created depending on context to validate transactions. Meant to be short-lived and non-reusable.

An overview of the ecosystem can be found at a categorization effort by TNO's eSSIF-lab [here](https://tno-ssi-lab.github.io/standardisation-overview/). Different layers of components are categorized according to their position in the ecosystem.

As our goal is to support the two use-cases above, we need:
- Digital identifiers for all entities in the use-cases. 
- Mock verifiable credentials for all real-world identity/certificate/visa
- Verifiable representations from the credentials containing the minimal representation of information needed by the verifying entities and the target/provee entities (entities being proven to).
- Zero-Knowledge Proof implementations that support minimization of data leaking.


### *Technology*

#### 1. Decentralized Identifiers

The DID specifications (latest version available [here](https://www.w3.org/TR/did-core/)) define the generic specifications for:
- what identifiers are supposed to define,
- how they are resolved,
- what functions and interfaces the underlying technologies should support, 
- definitions of a wallet registry service and 

An example of such an identifier:
![ExampleDID](../assets/zkp_ssi_1/ExampleDID.png)

The actual DID document is to be written using JSON-LD (if @context is included) or JSON. Here's an example of a DID document with a single verification method type, taken from the specifications.
```json
{
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/ed25519-2020/v1"
    ],
    "id": "did:example:123",
    "authentication": [
	    "did:example:123456789abcdefghi#keys-1",
      {
        "id": "did:example:123#z6MkecaLyHuYWkayBDLw5ihndj3T1m6zKTGqau3A51G7RBf3",
        "type": "Ed25519VerificationKey2020", // external (property value)
        "controller": "did:example:123",
        "publicKeyMultibase": "zAKJP3f7BD6W4iWEQ9jwndVTCBq8ua2Utt8EEjJ6Vxsf"
      }
    ],
    "capabilityInvocation": [
      {
        "id": "did:example:123#z6MkhdmzFu659ZJ4XKj31vtEDmjvsi5yDZG5L7Caz63oP39k",
        "type": "Ed25519VerificationKey2020", // external (property value)
        "controller": "did:example:123",
        "publicKeyMultibase": "z4BWwfeqdp1obQptLLMvPNgBw48p7og1ie6Hf9p5nTpNN"
      }
    ],
    "capabilityDelegation": [
      {
        "id": "did:example:123#z6Mkw94ByR26zMSkNdCUi6FNRsWnc2DFEeDXyBGJ5KTzSWyi",
        "type": "Ed25519VerificationKey2020", // external (property value)
        "controller": "did:example:123",
        "publicKeyMultibase": "zHgo9PAmfeoxHG8Mn2XHXamxnnSwPpkyBHAMNF3VyXJCL"
      }
    ],
    "assertionMethod": [
      {
        "id": "did:example:123#z6MkiukuAuQAE8ozxvmahnQGzApvtW7KT5XXKfojjwbdEomY",
        "type": "Ed25519VerificationKey2020", // external (property value)
        "controller": "did:example:123",
        "publicKeyMultibase": "z5TVraf9itbKXrRvt2DSS95Gw4vqU3CHAdetoufdcKazA"
      }
    ]
}
```

This is just an example: you need to choose two main aspects for a DID:
1. **Method**: There's a large spectrum (hundreds) of DID methods, all in varying degrees of maturity. A more comprehensive listing is found at [didregistry.com](didregistry.com). Companies developing solutions around SSI and DIDs often use the following:
	1. **Ethereum**: *did:eth* - DIDs resolvable through the Ethereum ecosystem
	2. **Public key**: *did:key* - Public Key DIDs that are usually used for local applications
	3. **Web**: *did:web* - Simple implementation of non-blockchain DIDs that can be hosted anywhere
	4. **Ion**: *did:ion* - DIDs on a Layer 2 network on top of the Bitcoin ecosystem
	5. **Sovrin**: *did:sov* - DIDs on the Sovrin network which is a private ledger based on Hyperledger Indy.
	6. **Indy**: *did:indy* - DIDs resolvable on any network based on Hyperledger Indy
	7. **Peer**: *did:peer* - An extensible version of did:keys that can be hyperlocal and usable by devices
	8. **Sidetree** - [*did:sidetree*](https://identity.foundation/sidetree/spec/) - A specification for how to support DIDs running atop any 'decentralized anchoring chain' (Bitcoin, Ethereum, other DLTs)
	
  Many more exist! All of these methods come with their own standards and specifications for interoperability. No form of versioning is consistent across these methods, as they are all independently developed (and correspondingly not always compatible with all the available tooling).

2. **Verification Methods**: DID documents usually mention one or more verification methods, such as cryptographic public keys to authenticate or authorize actions based on the DID. Verification methods can take an arbitrary number of parameters, depending on the implementation. A overview is found [here](https://www.w3.org/TR/did-spec-registries/), which also provides input for all possible values supported by DID documents, as well as many exceptional use-cases.

A DID document may contain references or definitions of the Verification Methods associated with the DID for the purposes of 
- *Authentication*: logging and general credential use
- *Assertion*: making claims, such as for issuing a Verifiable Credential
- *KeyAgreement*: encrypting data for transit
- *CapabilityInvocation* and *CapabilityDelegation*: Invoking and Delegating specific capabilities beyond authentication or assertion - It is yet unclear what a Capability could be described as, but they seem to include updating the various parts of the DID document itself.

In addition, there's a few concepts that are interesting towards building an ecosystem surrounding DIDs.

1. **DIDComm v2** - Protocol stack/Messaging standard supporting libraries for various different types of interactions for and between DID entities. These are HTTP or WS based API protocol specifications defining process flows for different scenarios such as issuing credentials and making data agreements. A serviceEndpoint must be defined in the DID.
2. **OIDC4SSI / SIOP** (Self-Issued OpenID Connect Provider) - Extending the OpenID flow to support Verifiable Credentials
3. **VC-JWT** - An extension of Verifiable Credentials to integrate with JWTs, which are a highly-used and stable web specification for authentication and authorization.


In our use-cases, our student Alice ends up having already a number of DIDs for different reasons, which is good because it allows them the chance to minimize the amount of information that external entities can gather about Alice.

![Overview of DIDs](../assets/zkp_ssi_1/OnlyDIDoverview.png)

However, since there are so many DIDs, Alice need a way to store all these credentials - enter Wallets. Wallets are a niche-group that many companies have entered - TNO's SSI lab has a very comprehensive list [here](https://github.com/tno-ssi-lab/wallet-overview). All these wallets have their own list of supported DID methods, and additional libraries on top to help manage DIDs, which can be very useful. We'll probably end up using one of them for convenience, but it's not too hard to roll our own!

We'll look into some of the wallets (and pick one for our implementation) in the next post in this series.

#### 2. Verifiable Credentials

While most of the ecosystem rightfully revolves around DIDs, these DIDs have to be associated with Credentials to be useful - and these are Verifiable Credentials (VCs). The latest specifications are found [here](https://www.w3.org/TR/vc-data-model/). 

VCs are a W3C standard, and the ecosystem is rapidly being constructed around them. VCs and DIDs are currently independent, and can be used without each other - VCs can work with non-DID URNs as well.

So what do VCs look like? The basic example from the specifications with annotations paints a good picture: -

```json
{
  // set the context, which establishes the special terms we will be using
  // such as 'issuer' and 'alumniOf'.
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://www.w3.org/2018/credentials/examples/v1"
  ],
  // specify the identifier for the credential
  "id": "http://example.edu/credentials/1872",
  // the credential types, which declare what data to expect in the credential
  "type": ["VerifiableCredential", "AlumniCredential"],
  // the entity that issued the credential
  "issuer": "https://example.edu/issuers/565049",
  // when the credential was issued
  "issuanceDate": "2010-01-01T19:23:24Z",
  // claims about the subjects of the credential
  "credentialSubject": {
    // identifier for the only subject of the credential
    "id": "did:example:ebfeb1f712ebc6f1c276e12ec21",
    // assertion about the only subject of the credential
    "alumniOf": {
      "id": "did:example:c276e12ec21ebfeb1f712ebc6f1",
      "name": [{
        "value": "Example University",
        "lang": "en"
      }, {
        "value": "Exemple d'Université",
        "lang": "fr"
      }]
    }
  },
  // digital proof that makes the credential tamper-evident
  // see the NOTE at end of this section for more detail
  "proof": {
    // the cryptographic signature suite that was used to generate the signature
    "type": "RsaSignature2018",
    // the date the signature was created
    "created": "2017-06-18T21:19:10Z",
    // purpose of this proof
    "proofPurpose": "assertionMethod",
    // the identifier of the public key that can verify the signature
    "verificationMethod": "https://example.edu/issuers/565049#key-1",
    // the digital signature value
    "jws": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..TCYt5X
      sITJX1CxPCT8yAV-TVkIEq_PbChOMqsLfRoPsnsgw5WEuts01mq-pQy7UJiN5mgRxD-WUc
      X16dUEMGlv50aqzpqh4Qktb3rk-BuQy72IFLOqV0G_zS245-kronKb78cPN25DGlcTwLtj
      PAYuNzVBAh4vGHSrQyHUdBBPM"
  }
}
```

In the use case for Alice above, the university creates a Verifiable Credential for their degree and adds identifiers for their DID (either user-provided or generated). Both governments follow a similar process for providing Passport Credentials and Visa Credentials respectively. The [Concrete Lifecycle example](https://www.w3.org/TR/vc-data-model/#concrete-lifecycle-example) from the official specifications is extremely useful to get a quick and clear idea of how such Credentials might work.

It is crucial that the identifiers are resolvable through the internet or through blockchains when more than one entity is involved.

Without diving too deep into a definition, let us mention a few aspects from the JSON above that are relevant for us -  
- **Issuer** - This is an entity that can be represented using a DID! In the example above, it's a URL resolved to the entity though.
- **CredentialSubject** - The most important part, the entity that the DID belongs to. This can be a DID too!
- **Type** - The type of Identifier - at this moment, specifications are still in development, so types of Credentials are still being developed (or used internally). This space could use some standardization of templates!
- **Proof** - VCs can be supported by proofs to ensure validity - these may be embedded or external proofs. Embedded proofs are based on Digital Signatures, as in the example above. External proofs follow the form of an JWT extension - more [here](https://w3c.github.io/vc-jwt/).
- **Status** - There are status schemes for the Verifiable Credential that can be extended based on the implementation. The status of a Verifiable Credential need not be part of it, and VCs can be revoked.

Two practical questions arise here:
- **Who holds the Verifiable Credential?** 
	
  The subject receives the Verifiable Credential. The issuer (university in our example) provides the subject with the Verifiable Credential JSON exactly once. This can be part of a wallet implementation, to store both DIDs and VCs as JSON documents. Verifiers can verify the Verify Credential's authenticity using the Digital Signature.
- **What details may (or may not) be part of a Verifiable Credential?**
	
  A fascinating question! There is a reference to this in the [Implementation Guidelines](https://www.w3.org/TR/vc-imp-guide/#zero-knowledge-proofs). It is possible for the Verifiable Credential to have all possible values (by extending the base structure) - and a new data model schema (like a JSON graph schema) must be created for this to be possible. 

	Clearly this isn't always possible - sometimes more complex data needs to be stored. For our example, we want to create something reasonably realistic. How might this be possible? Currently, the only possibility seems to be through links to external sources i.e. images, PDFs, databases, and so on.
	These links may be part of the the VC, and access could be limited to those entities that can prove ownership of both the VC and the subject DID (if present).

The next aspect of this is Verifiable Presentations - described below. We're almost at the point of making an architecture diagram and our initial specifications! We'll run/code some code in the next part of this blog-post.

#### 3. Verifiable Presentations

Verifiable Credentials may be signed and packaged within short-lived Verifiable Presentations (VPs) to prove ownership of such credentials. It's probably best to quote directly from the specifications here:

> Presentations may be used to combine and present credentials. They can be packaged in such a way that the authorship of the data is verifiable. The data in a presentation is often all about the same subject, but there is no limit to the number of subjects or issuers in the data. The aggregation of information from multiple verifiable credentials is a typical use of verifiable presentations.

Importantly, a Verifiable Presentation should be:
- contextual and short-lived
- bound to a challenge
- contain within them the Verifiable Credential(s), or a reference to the same with reduced content (in the case of Zero-Knowledge Proofs)
In a more generic sense, VPs prove authorship of the VC(s), through digital signature.

The 4 parts of a Verifiable Presentation (graph below implying that the content is represented in a semantic graph of the Linked Data JSON-LD format) are:
- Presentation metadata
- Credential graph
- Credential proof graph
- Presentation proof graph

An example of a Verifiable Presentation based on our example VC above:

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://www.w3.org/ns/credentials/examples/v2"
  ],
  "type": "VerifiablePresentation",
  // the verifiable credential issued in the previous example
  "verifiableCredential": [{
          "@context": [
            "https://www.w3.org/ns/credentials/v2",
            "https://www.w3.org/ns/credentials/examples/v2"
          ],
          "id": "http://university.example/credentials/1872",
          "type": ["VerifiableCredential", "ExampleAlumniCredential"],
          "issuer": "https://university.example/issuers/565049",
          "validFrom": "2010-01-01T19:23:24Z",
          "credentialSubject": {
            "id": "did:example:ebfeb1f712ebc6f1c276e12ec21",
            "alumniOf": {
              "id": "did:example:c276e12ec21ebfeb1f712ebc6f1",
              "name": "Example University"
            }
          },
          "proof": {
            "type": "DataIntegrityProof",
            "cryptosuite": "eddsa-2022",
            "created": "2023-06-18T21:19:10Z",
            "proofPurpose": "assertionMethod",
            "verificationMethod": "https://university.example/issuers/565049#key-1",
            "proofValue": "zQeVbY4oey5q2M3XKaxup3tmzN4DRFTLVqpLMweBrSxMY2xHX5XTYV8nQA
              pmEcqaqA3Q1gVHMrXFkXJeV6doDwLWx"
          }
  }],
  // digital signature by the issuer on the presentation
  // protects against replay attacks
  "proof": {
    "type": "DataIntegrityProof",
    "cryptosuite": "eddsa-2022",
    "created": "2018-09-14T21:19:10Z",
    "proofPurpose": "authentication",
    "verificationMethod": "did:example:ebfeb1f712ebc6f1c276e12ec21#keys-1",
    // 'challenge' and 'domain' protect against replay attacks
    "challenge": "1f44d55f-f161-4938-a659-f8026467f126",
    "domain": "4jt78h47fh47",
    "proofValue": "zqpLMweBrSxMY2xHX5XTYV8nQAJeV6doDwLWxQeVbY4oey5q2pmEcqaqA3Q1
      gVHMrXFkXM3XKaxup3tmzN4DRFTLV"
  }
}
```

While we understand the structure, this might become much clearer while implementing things.

### Recap

To recap, let's look at the architecture and flows so far:

Here's a simplified version with just a part of Case 1. 
- Alice can have many DIDs for different types of interactions in the ecosystem. 
- The University provides DIDs for all associated entities with a certain method and proving mechanism, and also one to Alice on admission.
- Alice is issued a VC by the university for the degree, with the subject of the VC being Alice's DID issued by the university.

![Case 1 Components](../assets/zkp_ssi_1/ComponentDiagramForStudentandUniversity.png)


Now, simplifying the many DIDs and VCs that an entity can have and combining both cases with all entities.
- Alice keeps VCs from both the governments and the univeristy in a digital wallet(s).
- VPs have to be created to make proofs of the VCs, such that they prove ownership, are short-lived and work for a particular use. In case 1, the VP is based on the single degree VC. In case 2, the VP is a combination of 2 VCs. In both cases, the companies know exactly only what is provided to them (but they are assured of the validity). In a next step, we will work on minimizing what we provide!
![Both case Components](../assets/zkp_ssi_1/SystemOverview.png)

Note that every entity shown here has their own digital wallet. It's possible that this could be an ecosystem of wallet-as-a-service companies.

In addition, there are 2 Verifiable Presentations created in our use cases. Before we dive into Zero-Knowledge Proofs and how they may apply to our use-case, let's consider the easier task of just signing and creating Presentations without selective disclosure. As you can see in the example VP above, the entire credential is embedded within the Presentation.

In the next part of this series, we'll dive into implementing the SSI aspects of this system, while mentioning some of the well-known and easy-to-use libraries out there.