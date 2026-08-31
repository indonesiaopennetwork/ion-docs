# Decentralized catalog and other changes in Beckn and impact on ION participants

## Introduction

In August 2026, NFH has changed Beckn 2.0 protocol. The changes make the protocol more decentralized and promotes self-publishing. This note explains the primary changes, provides references and discusses the impact on ION participants.

The changes are in two primary areas. The first is related self-hosted subscriber record and onboarding. The second is in self-hosted Catalogs. 

## Self hosted subscriber record and onboarding

Prior to this change, any NP needing to onboard onto a Beckn network (such as ION), had to signup to Dedi (dedi.global), create a namespace (related to the domain name of the NP), verify control over the domain and then create Beckn Subscriber registry and a record within it. The NFO (e.g. ION) would then link to this record to indicate the NP is member of the network. (In order to reduce friction for early stages of development, ION used to do this whole process within its own namespace. For production, the NP had to follow the above process)

With the new change, NP can onboard itself without having to signup for Dedi (NPs have an option to go the dedi.global route if they wish). The way the file based onboarding process is intended to work is as follows.
1. NP creates a `/.well-known/dedi.index.json` file at its domain (e.g. https://www.example.com/.well-known/dedi.index.json). This file contains a link to other registry json files.
2. NP create a Beckn Subscriber Registry file and a record within it.
3. NFO in its Reference registry adds a link to the registy that is self-hosted by NP.
4. The fabric registry service scans the link at the NFO reference registry and directs DeDi to crawl the domain. DeDi crawls the domain and adds the NP subscriber record to make it available for lookup. 
5. The rest of the process with Registry lookup by NP ONIX etc remain the same. 

More details on the new design can be [found here](https://docs.nfh.global/build/onboarding/file-based)

### Impact on ION participants.
1. As mentioned above, to reduce friction for ION participants in staging,  ION was already creating the subscriber record on behalf of the NP within its own namespace. This will continue to work as before. So nothing needs to be changed by NP for staging operations. 
2. When NPs need to go to production, they have an option to either create their subscriber record using the new file-based onboarding or use the dedi.global server to do it in an interactive way. ION will instruct the NP through the process at time of Key Creation for Production. 

## Self hosted catalog

Prior to the new self-hosted catalog, Seller Apps (Provider Node-PN) used to use the catalog/publish API call of the Catalog Server to publish their catalogs. Any discover service that was interested in the catalog and changes to it, would use the catalog/subscription and related endpoints of the Catalog Service to subscribe to it. The Catalog Service would use the push API endpoint of the Discover service to send the Catalog changes.

The new design brings the following:
1. The Catalog Service actor has been removed. There is no single/fabric Catalog Service anymore. 
2. The Seller App (PN) publishes the catalog and any changes to it on its own website. It also publishes an index file which is updated on every change to the catalog. It further publishes the path to this index file in the Beckn Subscriber Record metadata. 
3. The Discovery service does the following:
- It gets the list of Provider Nodes in the network and the path to their catalog index file. 
- It fetches the index file and identifies the difference between its view of the catalog and that of the Provider Node.
- If required, it crawls the Catalog and its changes and updates its own index.
4. This does not affect the discover/on_discover call. 
5. More details about this change can be [found here](https://docs.nfh.global/build/creating-a-network/catalog-publishing-and-discovery)

### Impact on ION participants
1. ION Participants acting as Seller Apps (Provider Nodes) will have to modify their publishing process. They will no longer be calling the fabric Catalog Service to publish. NFH has provided tooling that allows NPs to use their existing catalog/publish calls to result in self-hosted catalog and index file creation. This can be accessed through the [NFH Starter Kit](https://github.com/beckn/starter-kit). [ION Testnet](https://github.com/indonesiaopennetwork/ion-onix/tree/main/testnet) has been updated to incorporate this and NPs can use it as a playground to understand the catalog publish process. 
2. ION Participants who were using the catalog/subscription method to get catalogs, will now have to adopt the crawler process. Along with the starter kit, NFH has provided library support to help both the new publishing and crawling. It can be [found here](https://github.com/beckn/catalog-core)
3. There are no changes required for Buyer Apps that relied on hosted ION discover service. 

## Conclusion
NFH has moved subscriber details and catalog to a more decentralized, self-hosted design. The changes impact ION participants and in case of the catalog, will have software change/development effort. This document provided a summary of the changes and references for additional information.
