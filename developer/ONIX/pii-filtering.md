## Introduction

This document details the PII(Personally Identifiable information) filtering feature present in ONIX and how it will be used within ION. This document is originally written to document the existing filter functionality and will evolve to add the actual implementation within the ION network. This document uses the terms ION Network and Beckn Network interchangeably. ION is based on the Beckn Protocol. 

## Context

ION relies heavily on OTel based Network Observability for a lot of its value added features. Some of these value added features include automated settlement, grievance management system etc. In particular these features rely on the Audit Log feature of Network Observability, where NPs sends all Beckn Messages to the Network Collector. This is where masking of PII comes in. We do not want any PII to be either on the wire between the NP and the Network Observability, nor be stored in Network Observability store. All PII should be filtered at the NP before the Beckn messages are sent to the Network Observability. The rest of this document assumes NPs run ONIX on their node and explains the filtering as implemented in ONIX.

## Filtering in ONIX

ONIX provides support for OTel based data collection. It has two audiences. One is the NP himself (node). The other is Network Observability (network). The Beckn message forwarding to NO falls under the later. It is part of the Audit logging feature.

When providing Audit Logging, ONIX provides control on what is to be sent. It is configured in the `config/audit-fields.yaml` file. Overall, there are two modes in which audit logs work. One is the full mode where entire payload is sent. Other is the selective mode where only white-listed fields are sent. **NPs should use full mode** for Network Observability on ION to work properly. In both the modes, PII filtering is done. So from the perspective of PII filtering, the modes do not have any impact.

### Two step process
PII filtering in ONIX is designed as a two step process.
1. There is the maskRules section where we create a list of property names and associate a pattern to them. Example: The configuration below categorizes the properties email, emailAddress, supportEmail, providerEmail all as of type email. The depth where these properties occur within the JSON packet does not matter. As long 

```
maskRules:
  - keys: [email, emailAddress, supportEmail, providerEmail]
    pattern: email
```
2. The second step defines what mask we apply to the patterns. We can use one of two types of masks:
  - **replace** - Replaces the value with given value (Total redaction with/without retaining structure - depending on the value provided)
  - **last4** - Replaces the value with only last four characters visible (Partial redaction)

So for example, the following configuration replaces values of all the various properties that we previously identified as email pattern with a redacted email address.

```
patterns:
  email:
    maskType: replace
    mask: "***@***.***"
```

## ION implementation for PII
This section will be enhanced with a link to the external document that describes all the various PII rules NPs should implement. This policy will be reflected in the audio-fields.yaml sample config file for people who use ONIX. The document as well as the configuration file will evolve as we add new attributes to the message Objects.

## Future work
- Working with the legal team, we need to come up with the first version of the document mentioned above referencing all the attributes we currently have in the specs. We should modify the ONIX configuration to reflect the same.

