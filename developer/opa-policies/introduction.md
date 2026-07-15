## Introuduction

This document is a concept level note for why and how to use OPA Policies in ION. As we implement these, it will be improved with links to additional documents. 

This document refers to ION Network and Beckn Network interchangeably. ION network is based on the Beckn protocol. 

## Why do we need OPA Policies and checker

Network participants conduct transactions on an ION network by exchanging JSON messages. These messages follow the protocol specification written in JSON-Schema. By using a JSON-Schema validator, we can ensure the JSON packet's structure, data type and specified constraints are as per the published protocol specs. However there are a couple of reasons why this Schema Validation is not sufficient.
  - JSON Schema is not a full fledged policy specification language. Its primary strength is in structure and type. 
  - Conditionals in JSON Schema while possible, are very tedious to write and read
  - With the Beckn-v2 introducing the concept of self-contained context.jsonld and corresponding attributes.yaml, the counter party JSON Schema validator is mostly redundant. If a NP intentionally overrides a rule, it is not caught by the counter party. 

Due to these reasons, we will use the OPA Policies to specify business policies that need to be adhered to by all NPs. The rules will be written in Rego. Beckn ONIX already has a OPAPolicyChecker plugin which supports checking of these rules when a packet arrives at an NP's ONIX server.

## Policy authoring

Policies as a whole will be written by the Schema and Flow authors. Policies are broadly at following levels. 
- Object level policies - These will be present in the Object's attribute.yaml file as extension attributes (custom keywords) and are generally skipped by the json-schema validators.
- Action level policies - These are policies that are specific to a particular Beckn action (e.g. init request should have fulfillment address)
- Cross action policies - These are policies that are applied across multiple actions. Currently it is not planned to include these in the NP evaluated list. They might be used by ION devlabs validation framework.

## Policy publishing

All the policies present in the schema and flow files are bundled together and published as a OPA Policy bundle. A manifest is created and publicly published as per the [process listed here](https://github.com/beckn/beckn-onix/tree/main/pkg/plugin/implementation/opapolicychecker#manifest-backed-policies). 

## Policy validation

Every NP should download these policies and ensure that every packet that passes into their system does not violate any rule. ONIX already has a OPA Policy Checker [detailed here](https://github.com/beckn/beckn-onix/tree/main/pkg/plugin/implementation/opapolicychecker). So all that NPs have to do to comply with the validation requirement is to configure the plugin and point the source of the policies to the Network manifest. The default ONIX config will already have this configured.

## Future directions
- For Cross action policies, there are two options.
  - We can use the payloadstore and along with some plumbing through another plugin, implement the cross-action flow.
  - For now the cross action policies can be used in the devlabs validation
- This document can be improved along the following directions:
  - Directions to policy authors on the best practices (both for readability and optimal execution). Maybe also a recipes document for known scenarios.
  - Exact details on the Policy publishing process
  - Manual configuration document for those who want to configure ONIX manually