## Introduction

This is a small note on NP information available with Network Observability.

## Context

Currently the main features of Network Observability are identifying NPs, their catalogs and the messages that flows between them. However there is one other aspect of NO which can help troubleshoot anomalies in the network. That is identifying the configuration of ONIX that the NP is using. This includes things such as plugins, their configuration, routes etc.

ONIX has added the first batch of this info as metrics that get reported to NO. It is [detailed here](https://github.com/beckn/beckn-onix/blob/main/pkg/plugin/implementation/otelsetup/OBSERVABILITY.md#network-orchestrator-visibility)

## What is captured
In the current implementation of ONIX, the following information are collected and can be exported to the network collector. 
1. Modules configured for a particular subscriber_id (BAP,BPP etc)
2. Standard Plugins configured in each module
3. Custom Plugins configured in each module

## What is not captured
1. Whether the version of ONIX is the one released by ION/NFH. 
2. Whether the standard plugins have been modified
3. Configuration values - Both ONIX level and plugin configurations

## Future work
1. Firstly the Network Observability module has to be enhanced to capture and show the already captured values. This might also involve change in the default configuration of ONIX given to NPs. [Feature request for this is here](https://github.com/indonesiaopennetwork/ion-monitor/issues/6)
2. The distinction between pre-built and custom-built ONIX can be made by calculating a content hash and reporting it. It has not been requested yet, but if found necessary, this is one option. 