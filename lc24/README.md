# Attested TLS workshop @ [Linaro Connect 2024](https://www.linaro.org/connect)

[**link to the recordings**](https://armltd.zoom.us/rec/share/89YzsxVzOEIgnG9I8wX1LGJEjEsz6yVr2pNiTS8zOBUS78RVVIJ8GKSOftPv9Nhi.75rwsyl7unw_dTVt) ( Passcode: *t#$^A5xi* )

May 16 2024, 8am-6pm

Melia Avenida America, Madrid, Spain [(map)](https://www.openstreetmap.org/#map=17/40.44593/-3.62721)

Room: TSC2 | Avila

## Participants

| Name | Organisation | Country | Remote/Onsite |
|--|--|--|--|
| Arnaud de Grandmaison | Arm | FR | Onsite |
| Arto Niemi | Huawei | FI | Onsite |
| Carsten Weinhold | Barkhausen Institut | DE | Remote |
| Guilhem Bryant | Arm | GB | Remote |
| Ionut Mihalcea | Arm | GB | Onsite |
| Jimmy Kjällman | Ericsson | FI | Onsite
| Leonardo Garcia | Linaro | BR | Onsite |
| Muhammad Usama Sardar | TU Dresden | DE | Onsite |
| Ondrej Smid | Ericsson | SE | Onsite |
| Paul Howard | Arm | GB | Onsite (and attending full Connect event) |
| Thomas Fossati | Linaro | CH | Onsite |
| Thore Sommer | Uni Kiel | DE | Onsite |
| Yuxuan Song | Inria | FR | Onsite |

## Agenda

| Who | Track | Title | Timing (CET) |
|--|--|--|--|
| Thomas Fossati & Muhammad Usama Sardar | Intro | 10000-foot view | 8:40 - 9:00 |
| Paul Howard | Use Cases | Role of attested TLS in service mesh | 9:20 - 9:40 |
| Paul Howard | Use Cases | Mutual attestation of cloud and edge nodes | 9:40 - 10:00 |
| Jimmy Kjällman & Ondrej Smid | Use Cases and Implementations | Ericsson use case and rusttls prototype | 9:00 - 9:20 |
| break | | | 10:00 - 10:20 |
| Guilhem Bryant | Use cases and Implementations | Using attested TLS to build a trusted cloud | 10:20 - 10:40 |
| Ionut Mihalcea | Implementations | mbedTLS prototype: present and future | 11:00 - 11:20 |
| Thomas Fossati | Spec work | Report | 11:20 - 11:40 |
| Muhammad Usama Sardar | FV | Challenges in the formal verification of attested TLS | 11:40 - 12:00 |
| lunch break | | | 12:00 - 14:00 |
| Hackathons (one per each track) | | | 14:00 - 18:00 |

## Minutes

### 10000-foot view    

[link to slides](https://github.com/tls-attestation/materials/blob/main/lc24/Intro.pdf)

- Confirmed that all participants are happy to share materials publicly both during and after the meeting.
- Thanks to Leonardo of Linaro Data Center group for helping with the organisation at short notice.
- Usama presents on the "10000 foot view" of attestation
    - Use cases
    - History
    - Collaboration model
- Usama stressed the importance of formal verification as an early part of the collaborative process
- ThomasF presented an introduction to the Attested TLS project, covering the concepts, status, early use cases, PoC architecture

### Ericsson use case and prototype

[link to slides](https://github.com/tls-attestation/materials/blob/main/lc24/attested_tls_use_cases_ericsson.pdf)

* Context: mobile network (6G)
* Computational offloading from UE (phones) to edge device (e.g., to spare energy)
* CPU-bound tasks, but not only
* Requirements:
    * TEE platform independence
    * Good performance (especially wrt latency)
    * Poliglot execution environment
* Integration with the mobile network authentication mechanics (AKMA), which is based on SIM credentials
    * a PSK is provisioned into the UE based on the authenticated SIM
* UEs are by definition mobile, so there is a need to allow the offloaded workload to migrate to "follow" the UE
* From a CC perspective, the edge node where the worload is offloaded is the TEE

These map to the following aTLS requirements/desiderata:
* PSK support
* mutual authentication
* lightweight instantiation

Paper at CLOSER 2024: ["Benefits of Dynamic Computational Offloading for Mobile Devices"](https://www.scitepress.org/PublicationsDetail.aspx?ID=ccKzjw4ElfA=&t=1)

### Role of attested TLS in service mesh

[link to slides](https://github.com/tls-attestation/materials/blob/main/lc24/attested_tls_use_cases_paulh.pdf)

*  Reduce the effort for microservices in cloud-native application
*  CNCF: Istio and envoy 
*  often deployed as sidecar
*  When microservices communicate through proxies: proxy takes "heavy lifting" of secure communication (TLS)
*  Proposing envoy to use OpenSSL (rather than BoringSSL): can it still maintain custom certificate validator?
*  Open-ended questions/discussion points to be discussed
*  Connection: Attested TLS might overlap or compete with SPIFFE/SPIRE (SPIFFE is standard, SPIRE is implementation)
*  Slides [here](https://github.com/tls-attestation/materials/blob/main/lc24/attested_tls_use_cases_paulh.pdf)
* Would SPIFFE be a better consumer of attestation, instead of exposing it to the proxies?
* We should have a side conversation, maybe at the next IETF, to bring together SPIFFE people + WIMSE people + aTLS people

### Mutual attestation of cloud and edge nodes

[link to slides](https://github.com/tls-attestation/materials/blob/main/lc24/attested_tls_use_cases_paulh.pdf)

* A type of merger of the 2 use cases: IoT and CC 
* Spanning the cloud as well as the edge
* PSA: Platform attestation
* Attested TLS in CCA - PSA 
* Open-ended questions/discussion points to be discussed
* AI at the edge: Other entities like application provider 

### Using attested TLS to build a trusted cloud

[link to slides](https://github.com/tls-attestation/materials/blob/main/lc24/dpu_use_case.pdf)

* Guilhem presenting
* Q for Guilhem: Is there some way to port our design work for aTLS to IPSec, as used in this project?
	* IPSec isn't used, it was one of the options during the investigation, but TLS was chosen instead
* Q from Guilhem: Thomas mentioned something about mutual attestation, but the PoC seems to be asymmetric in this regard?
	* The draft allows for mutual attestation, while the PoC is currently more restricted in its abilities.
* DPU to host attestation flow seems inverted to usual TDISP? Does it need an extension in TDISP? Does aTLS carry evidence for both DPU or DPU+Host, and does this have a bearing on TDISP?
	* TDISP is only between Host and DPU
	* There seems to be a need for the whole bundle of DPU + Host to attest itself to external entitites; usually TDISP is used by the Host to grab some evidence to be conveyed to external parties
* Comparing aTLS with aCSR (as the original Veracruz approach): would be interested to see the freshness vs performance trade-off and some benchmarking around it.
	* Performance-wise, aTLS might be more comparable in the passport model use-case
	* Could align with Carsten / Michael and compare numbers

### Implementation of Attested TLS using PSK in rustls

[link to slides](https://github.com/tls-attestation/materials/blob/main/lc24/attested_tls_use_cases_ericsson.pdf)

- Jimmy and Ondrej presenting
- Builds on RFC9258 for pre-shared key (PSK)
- Sequence diagram presented
- Concern about the cost of sequential operations - any opportunities to parallelise?
- Overheads come mostly from the active work to generate the evidence and do verifications, rather than any inherent cost in adding these payloads into the handshake
- Background check is not necessarily best for performance
- For mutual attestation, it might be possible to do client and server verification in parallel
- Likewise for evidence generation
- Different permutations of background check and passport models possible on client and server
- Rustls prototype is currently not public. It is possible (but not certain) that it will become public later
- BS thesis by Leon Hejdenberg Philip: ["Establishment of Secure Channel Binding with Remote Party Attestation"](https://uu.diva-portal.org/smash/get/diva2:1852751/FULLTEXT01.pdf)

### mbedTLS prototype: present and future

[link to slides](https://github.com/tls-attestation/materials/blob/main/lc24/mbedtls_prototype.pdf)

- Ionut presenting
- Placeholder for [link to slides]()
- The PoC embodies the RATS architecture, with concrete focus on attester and relying party, with attested TLS handshake between them
- The PoC is flexible, but it is expensive to implement all possible patterns, so the PoC focused on one concrete pattern
- The client is the attester and the server is the relying party
- Background check model is used
- On both client and server side, we have implemented some components in Rust but wrapped in C FFI to enable call-outs from mbed TLS (which is written in C)
- There were two independent forks of mbedTLS by Ionut and PaulH, acting as attester (with Parsec integration) and relying party (with Veraison integration), but these have been brought together into a common branch as part of Guilhem's work
- There has been some bit rot due to lack of version pinning
- Veraison interfaces evolving and we haven't caught up
- Negotiation is not yet implemented, neither is passport model
- Planning to support different RoT technologies such as Arm CCA

### Spec work status update

[link to slides](https://github.com/tls-attestation/materials/blob/main/lc24/standardisation-recap.pdf)

### Challenges in the formal verification of attested TLS

[link to slides](https://github.com/tls-attestation/materials/blob/main/lc24/Formalization-Usama.pdf)

- Challenges in specification 
    - Incomplete and outdated specs for Intel's RA-TLS
- Challenges from formal perspective 
    - Very few comments in Inria's formal model
    - Incomplete validation of draft 20 artifacts
    - Not easily extensible
- [link to slides](https://github.com/tls-attestation/materials/blob/main/lc24/Formalization-Usama.pdf)
- Do all implementations that rely on RA-TLS use TLS v1.2 (as specified)?
	- no, they've actually ported it to v1.3, despite no updated spec
- "No result" outcome from ProVerif is not an explicit signal, just implicit from the fact that the run seems to never finish
- What kind of deliverables do we need for FV? 
	- Are we going to have a single model, or multiple models (one for each mode of operation)?
		- Not thought about it. Ideally it would be good to have one model only, but it might not be possible to scale the tools to the whole array of options.

# Hackathon Reports

## Envoy Proxy Extension (PaulH)

Thesis/question: Is it possible to create a custom cert validator in Envoy Proxy to treat the cert as an attestation credential, and call a verifier such as Veraison to evaluate it?

Based on the assumption that at least one custom validator already exists for SPIFFE identity documents (SVID).

Findings...

Not a trivial "afternoon" job, unfortunately. This is because the extensibility mechanisms that exist within Envoy do not treat the cert as an arbitrary blob. It is already assumed to be an x509 chain.

The cert validator interface contract is [here](https://github.com/envoyproxy/envoy/blob/c45a419bf86ae5db738da1aa28d1887b32c4dfdb/source/common/tls/cert_validator/cert_validator.h#L46).

The actual method to perform validation is `doVerifyCertChain()` [here](https://github.com/envoyproxy/envoy/blob/c45a419bf86ae5db738da1aa28d1887b32c4dfdb/source/common/tls/cert_validator/cert_validator.h#L82).

Note that the input is `STACK_OF(X509)`.

Since it's a typed X509 chain, it would not be possible to pass this to Veraison as a raw attestation credential.

I was misremembering that SPIFFE SVIDs were actually JWT rather than X509, but there are actually two flavours of SVID, one of which is JWT and the other is X509, so clearly the Envoy validator is able to use the X509 format and conform with the existing interface. For something like an CMW-wrapped attestation credential, we wouldn't have that luxury.

Seems likely that we would have to go deeper into the Envoy stack to find a more primitive branching point, before the X509 assumptions are made.

The call point of `doVerifyCertChain()` is [here](https://github.com/envoyproxy/envoy/blob/ab99cd3725a690c69146decb7490cb7e1fd96a59/source/common/tls/context_impl.cc#L507).

The source of the X509 chain is the BoringSSL API `SSL_peer_get_full_cert_chain()` [here](https://commondatastorage.googleapis.com/chromium-boringssl-docs/ssl.h.html#SSL_get_peer_full_cert_chain).

This API just returns `NULL` if the peer is using a non-X509 credential, so there's no way with BoringSSL to get other types of credential as raw bytes.

Hence probably needs swapping out BoringSSL and bringing in OpenSSL and making use of the work of Carsten's team.

## Spec work

Triaged existing issues, created new issues, assigned issues to the [Vancouver milestone](https://github.com/tls-attestation/draft-tls-attestation/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22Vancouver+%28was%3A+Brisbane%29%22).

