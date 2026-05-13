## Introduction

ION network is based on Beckn v2 Schema. Beckn v2 provides a field of type Attributes in all major Objects (e.g. Resource, Contract, Provider etc). This field is called resourceAttributes, contractAttributes, providerAttributes etc. This provides a place where one can provide a context, type (and there by an attributes.yaml) and extend the object. It is intended for extension by networks such as ION as well as any participant on the network.


ION wanted to use these Attributes to define the core fields required by the contained object. We had the following objectives:
1. All extension Attributes would be defined by ION (e.g. IONTradeResourceAttributes, IONContractAttributes etc). Do not allow NP to provide own context at the core extension points (resourceAttributes, contractAttributes etc)
2. This object would have all the fields relevant to the sector/usecase.
3. We will keep this object closed and not allow additional properties. The objective here was to avoid confusion due to NP added properties conflicting with similar sounding existing properties.
4. Within each of these objects we will provide an attribute called participantAttributes(or extraAttributes). This would follow the same structure as the Beckn Attributes object. It will have context and type which can be used by NP to specify structure for the attributes the NP is adding. All of these additional properties added by NP will come under participantAttributes and not in the same level as ION defined properties.


## Previous design
In order to achieve the above objectives, we had proposed doing the following:
1. Within the Beckn Attributes object, limit the context to be only from ION (through regex/rego). This would ensure all extension points are only ION Classes
2. A new object called IONExtraAttributes was created with identical structure as Beckn Attributes object.
3. The ION created extension classes (such as TradeResourceAttributes) would be closed for additionalProperties. It would have a property called participantAttributes(or extraAttributes) which is of type IONExtraAttributes. This is the place where NPs add their own properties.

### Pushback
When we requested comments on this approach, the following were noted.
1. Limiting only ION Objects to appear in core extension properties(resourceAttributes,contractAttributes) enforced through rego/regex was found to be unnecessary (NP overriding through own classes was not found to be a significant problem)
2. Enforcing ION classes at all extension points in one swoop was found to be excessive. 

## New design
The new design is mostly a unwind of the rego/regex rule, enforcing NP properties extension from one place and creation of new IONExtraAttributes object etc required to achieve those. The following are the key elements.
1. Individual ION defined extension schema objects (e.g. IONTradeResource, IONContractResource) will add all fields required for its operation. Further the schema object creator can decide whether to allow additionalProperties at this level or not. The recommended approach is to set additionalProperties as false and create a field called participantProperties. This will have same structure as Attributes object of Beckn (context, type etc)
2. If a ION defined object has participantProperties field, NPs can use those to define their own properties. They will have to provide a context when they add fields. They can also add additional fields not covered by context/attributes.yaml. However all the NP added fields is recommended to be at this level and not in the parent. 

The key takeaway is that we have removed the one-place driven uniform approach for NP extensions and have replaced it with recommendations for each ION Extension Object designer to follow. It achieves most of our initial objectives. The only objective not addressed by this design is one where we wanted to prevent NPs from overriding core ION Objects.

## Implementation test results

This section gives sample json packets, that show the new design in action. There is a postman collection in this folder with these requests. Also the required example contexts are defined in the ionAttributes folder under two subfolders 
1. TradeResourceAttributes - ION defined
2. ParticipantAttributes - Participant defined

### Base Beckn Retail resource used for this experiment
- This is a sample taken from the Local-Retail repo with just bare minimum publish request
- It passes validation without any issue
```
{
  "context": {
    "version": "{{version}}",
    "action": "catalog/publish",
    "timestamp": "{{$isoTimestamp}}",
    "messageId": "{{$guid}}",
    "transactionId": "{{$randomUUID}}",
    "bppId": "{{bppId}}",
    "bppUri": "{{bppUri}}",
    "ttl": "PT30S",
    "networkId": "{{networkId}}"
  },
"message": {
    "catalogs": [
      {
        "id": "catalog-hbo-001",
        "bppId": "bpp.example.com",
        "bppUri": "http://onix-bpp:8082/bpp/receiver",
        "descriptor": {
          "name": "HBO",
          "shortDesc": "Electronics and home goods catalog"
        },
        "provider": {
          "id": "provider-venky-bazaar",
          "descriptor": {
            "name": "Venky.Mahadevan@Bazaar"
          }
        },
        "validity": {
          "startDate": "2023-01-01T00:00:00Z",
          "endDate": "2023-12-31T23:59:59Z"
        },
        "resources": [
          {
            "id": "item-flask-mh500-yellow",
            "descriptor": {
              "name": "Isothermal Stainless Steel Hiking Flask MH500 Yellow",
              "shortDesc": "Double walled vacuum insulated stainless steel thermos flask, 500ml"
            },
            "resourceAttributes": {
              "@context": "https://raw.githubusercontent.com/beckn/local-retail/refs/heads/main/schema/RetailResource/v2.1/context.jsonld",
              "@type": "RetailResource"
            }
          }
        ],
        "offers": [
          {
            "id": "offer-flask-mh500-yellow",
            "descriptor": {
              "name": "Isothermal Stainless Steel Hiking Flask MH500 Yellow"
            },
            "resourceIds": [
              "item-flask-mh500-yellow"
            ],
            "provider": {
              "id": "provider-venky-bazaar",
              "descriptor": {
                "name": "Venky.Mahadevan@Bazaar"
              }
            },
            "validity": {
              "startDate": "2023-01-01T00:00:00Z",
              "endDate": "2023-12-31T23:59:59Z"
            },
            "offerAttributes": {
              "@context": "https://raw.githubusercontent.com/beckn/local-retail/refs/heads/main/schema/RetailOffer/v2.1/context.jsonld",
              "@type": "RetailOffer"
            }
          }
        ]
      }
    ]
  }
}
```

### TradeResource class usage with no extra fields.
- This uses the ION defined TradeResource schema object(Note the context in the resourceAttributes field). There the countryOfOrigin is a required field. 
- This passes validation without error.

```
{
  "context": {
    "version": "{{version}}",
    "action": "catalog/publish",
    "timestamp": "{{$isoTimestamp}}",
    "messageId": "{{$guid}}",
    "transactionId": "{{$randomUUID}}",
    "bppId": "{{bppId}}",
    "bppUri": "{{bppUri}}",
    "ttl": "PT30S",
    "networkId": "{{networkId}}"
  },
"message": {
    "catalogs": [
      {
        "id": "catalog-hbo-001",
        "bppId": "bpp.example.com",
        "bppUri": "http://onix-bpp:8082/bpp/receiver",
        "descriptor": {
          "name": "HBO",
          "shortDesc": "Electronics and home goods catalog"
        },
        "provider": {
          "id": "provider-venky-bazaar",
          "descriptor": {
            "name": "Venky.Mahadevan@Bazaar"
          }
        },
        "validity": {
          "startDate": "2023-01-01T00:00:00Z",
          "endDate": "2023-12-31T23:59:59Z"
        },
        "resources": [
          {
            "id": "item-flask-mh500-yellow",
            "descriptor": {
              "name": "Isothermal Stainless Steel Hiking Flask MH500 Yellow",
              "shortDesc": "Double walled vacuum insulated stainless steel thermos flask, 500ml"
            },
            "resourceAttributes": {
              "@context": "https://raw.githubusercontent.com/indonesiaopennetwork/ion-docs/refs/heads/tradeResourceAttributes/drafts/ionSchema/ionAttributes/resourceAttributes/TradeResourceAttributes/v1.0/context.jsonld",
              "@type": "TradeResource",
              "countryOfOrigin": "Indonesia"
            }
          }
        ],
        "offers": [
          {
            "id": "offer-flask-mh500-yellow",
            "descriptor": {
              "name": "Isothermal Stainless Steel Hiking Flask MH500 Yellow"
            },
            "resourceIds": [
              "item-flask-mh500-yellow"
            ],
            "provider": {
              "id": "provider-venky-bazaar",
              "descriptor": {
                "name": "Venky.Mahadevan@Bazaar"
              }
            },
            "validity": {
              "startDate": "2023-01-01T00:00:00Z",
              "endDate": "2023-12-31T23:59:59Z"
            },
            "offerAttributes": {
              "@context": "https://raw.githubusercontent.com/beckn/local-retail/refs/heads/main/schema/RetailOffer/v2.1/context.jsonld",
              "@type": "RetailOffer"
            }
          }
        ]
      }
    ]
  }
}
```

### Adding extra fields at the top level
- This packet adds extra fields at the TradeResouce object level. Since this is closed for additionalProperties at this level, it fails. Error is pasted below the packet

```
{
  "context": {
    "version": "{{version}}",
    "action": "catalog/publish",
    "timestamp": "{{$isoTimestamp}}",
    "messageId": "{{$guid}}",
    "transactionId": "{{$randomUUID}}",
    "bppId": "{{bppId}}",
    "bppUri": "{{bppUri}}",
    "ttl": "PT30S",
    "networkId": "{{networkId}}"
  },
"message": {
    "catalogs": [
      {
        "id": "catalog-hbo-001",
        "bppId": "bpp.example.com",
        "bppUri": "http://onix-bpp:8082/bpp/receiver",
        "descriptor": {
          "name": "HBO",
          "shortDesc": "Electronics and home goods catalog"
        },
        "provider": {
          "id": "provider-venky-bazaar",
          "descriptor": {
            "name": "Venky.Mahadevan@Bazaar"
          }
        },
        "validity": {
          "startDate": "2023-01-01T00:00:00Z",
          "endDate": "2023-12-31T23:59:59Z"
        },
        "resources": [
          {
            "id": "item-flask-mh500-yellow",
            "descriptor": {
              "name": "Isothermal Stainless Steel Hiking Flask MH500 Yellow",
              "shortDesc": "Double walled vacuum insulated stainless steel thermos flask, 500ml"
            },
            "resourceAttributes": {
              "@context": "https://raw.githubusercontent.com/indonesiaopennetwork/ion-docs/refs/heads/tradeResourceAttributes/drafts/ionSchema/ionAttributes/resourceAttributes/TradeResourceAttributes/v1.0/context.jsonld",
              "@type": "TradeResource",
              "countryOfOrigin": "Indonesia",
              "countryOfDesign": "India"
            }
          }
        ],
        "offers": [
          {
            "id": "offer-flask-mh500-yellow",
            "descriptor": {
              "name": "Isothermal Stainless Steel Hiking Flask MH500 Yellow"
            },
            "resourceIds": [
              "item-flask-mh500-yellow"
            ],
            "provider": {
              "id": "provider-venky-bazaar",
              "descriptor": {
                "name": "Venky.Mahadevan@Bazaar"
              }
            },
            "validity": {
              "startDate": "2023-01-01T00:00:00Z",
              "endDate": "2023-12-31T23:59:59Z"
            },
            "offerAttributes": {
              "@context": "https://raw.githubusercontent.com/beckn/local-retail/refs/heads/main/schema/RetailOffer/v2.1/context.jsonld",
              "@type": "RetailOffer"
            }
          }
        ]
      }
    ]
  }
}
```
- ONIX catches the invalid field
```
{
    "message": {
        "ack": {
            "status": "NACK"
        },
        "error": {
            "code": "Bad Request",
            "paths": "message.catalogs[0].resources[0].resourceAttributes.properties",
            "message": "property \"countryOfDesign\" is unsupported"
        }
    }
}
```

### Adding extra fields at ProviderAttributes level
- TradeResource allows NPs to add extra fields within the providerAttributes property. 
- This passes validation as extension is allowed there.

```
{
  "context": {
    "version": "{{version}}",
    "action": "catalog/publish",
    "timestamp": "{{$isoTimestamp}}",
    "messageId": "{{$guid}}",
    "transactionId": "{{$randomUUID}}",
    "bppId": "{{bppId}}",
    "bppUri": "{{bppUri}}",
    "ttl": "PT30S",
    "networkId": "{{networkId}}"
  },
"message": {
    "catalogs": [
      {
        "id": "catalog-hbo-001",
        "bppId": "bpp.example.com",
        "bppUri": "http://onix-bpp:8082/bpp/receiver",
        "descriptor": {
          "name": "HBO",
          "shortDesc": "Electronics and home goods catalog"
        },
        "provider": {
          "id": "provider-venky-bazaar",
          "descriptor": {
            "name": "Venky.Mahadevan@Bazaar"
          }
        },
        "validity": {
          "startDate": "2023-01-01T00:00:00Z",
          "endDate": "2023-12-31T23:59:59Z"
        },
        "resources": [
          {
            "id": "item-flask-mh500-yellow",
            "descriptor": {
              "name": "Isothermal Stainless Steel Hiking Flask MH500 Yellow",
              "shortDesc": "Double walled vacuum insulated stainless steel thermos flask, 500ml"
            },
            "resourceAttributes": {
              "@context": "https://raw.githubusercontent.com/indonesiaopennetwork/ion-docs/refs/heads/tradeResourceAttributes/drafts/ionSchema/ionAttributes/resourceAttributes/TradeResourceAttributes/v1.0/context.jsonld",
              "@type": "TradeResource",
              "countryOfOrigin": "Indonesia",
              "participantAttributes": {
                "@context": "https://raw.githubusercontent.com/indonesiaopennetwork/ion-docs/refs/heads/tradeResourceAttributes/drafts/ionSchema/ionAttributes/resourceAttributes/ParticipantAttributes/v1.0/context.jsonld",
                "@type": "ParticipantResource",
                "countryOfDesign": "India"
              }
            }
          }
        ],
        "offers": [
          {
            "id": "offer-flask-mh500-yellow",
            "descriptor": {
              "name": "Isothermal Stainless Steel Hiking Flask MH500 Yellow"
            },
            "resourceIds": [
              "item-flask-mh500-yellow"
            ],
            "provider": {
              "id": "provider-venky-bazaar",
              "descriptor": {
                "name": "Venky.Mahadevan@Bazaar"
              }
            },
            "validity": {
              "startDate": "2023-01-01T00:00:00Z",
              "endDate": "2023-12-31T23:59:59Z"
            },
            "offerAttributes": {
              "@context": "https://raw.githubusercontent.com/beckn/local-retail/refs/heads/main/schema/RetailOffer/v2.1/context.jsonld",
              "@type": "RetailOffer"
            }
          }
        ]
      }
    ]
  }
}
```
- With more additional attributes.
```
{
  "context": {
    "version": "{{version}}",
    "action": "catalog/publish",
    "timestamp": "{{$isoTimestamp}}",
    "messageId": "{{$guid}}",
    "transactionId": "{{$randomUUID}}",
    "bppId": "{{bppId}}",
    "bppUri": "{{bppUri}}",
    "ttl": "PT30S",
    "networkId": "{{networkId}}"
  },
"message": {
    "catalogs": [
      {
        "id": "catalog-hbo-001",
        "bppId": "bpp.example.com",
        "bppUri": "http://onix-bpp:8082/bpp/receiver",
        "descriptor": {
          "name": "HBO",
          "shortDesc": "Electronics and home goods catalog"
        },
        "provider": {
          "id": "provider-venky-bazaar",
          "descriptor": {
            "name": "Venky.Mahadevan@Bazaar"
          }
        },
        "validity": {
          "startDate": "2023-01-01T00:00:00Z",
          "endDate": "2023-12-31T23:59:59Z"
        },
        "resources": [
          {
            "id": "item-flask-mh500-yellow",
            "descriptor": {
              "name": "Isothermal Stainless Steel Hiking Flask MH500 Yellow",
              "shortDesc": "Double walled vacuum insulated stainless steel thermos flask, 500ml"
            },
            "resourceAttributes": {
              "@context": "https://raw.githubusercontent.com/indonesiaopennetwork/ion-docs/refs/heads/tradeResourceAttributes/drafts/ionSchema/ionAttributes/resourceAttributes/TradeResourceAttributes/v1.0/context.jsonld",
              "@type": "TradeResource",
              "countryOfOrigin": "Indonesia",
              "participantAttributes": {
                "@context": "https://raw.githubusercontent.com/indonesiaopennetwork/ion-docs/refs/heads/tradeResourceAttributes/drafts/ionSchema/ionAttributes/resourceAttributes/ParticipantAttributes/v1.0/context.jsonld",
                "@type": "ParticipantResource",
                "countryOfDesign": "India",
                "countryOfAssembly": "China"
              }
            }
          }
        ],
        "offers": [
          {
            "id": "offer-flask-mh500-yellow",
            "descriptor": {
              "name": "Isothermal Stainless Steel Hiking Flask MH500 Yellow"
            },
            "resourceIds": [
              "item-flask-mh500-yellow"
            ],
            "provider": {
              "id": "provider-venky-bazaar",
              "descriptor": {
                "name": "Venky.Mahadevan@Bazaar"
              }
            },
            "validity": {
              "startDate": "2023-01-01T00:00:00Z",
              "endDate": "2023-12-31T23:59:59Z"
            },
            "offerAttributes": {
              "@context": "https://raw.githubusercontent.com/beckn/local-retail/refs/heads/main/schema/RetailOffer/v2.1/context.jsonld",
              "@type": "RetailOffer"
            }
          }
        ]
      }
    ]
  }
}
```