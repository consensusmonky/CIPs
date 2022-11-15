---
CIP: 78
Title: A standard for smart and programmable NFTs
Authors: Phillip Sailer <@consensusmonky, @DIENAY_IO>
Comments-URI:
Status: Draft
Type: Process
Created: 2022-11-15
License: CC-BY-4.0
---

The following CIP addresses a standard for "functional" NFTs (iNFTs, smart NFTs) that extends the idea of CIP-0068.
The idea behind this standard is to have some kind of small digital modules that can be easily integrated with other business logic that rely on secure ownership data. The transition to digital property and Web3 requires a simple way to integrate, use, and communicate between digital assets and any software that needs to interact with them. The simplest example would be the metaverse or the gaming industry. But there are enough other industries as well.  A common language in the DLT world helps us develop and integrate NFTs in general. Whether in real life or in the metaverse.
It uses the basic concepts from CIP-31 to CIP-33.

## Preamble

NFTs in general are one of the most, if not the most, well-known asset classes based on DLT today. Regardless of which blockchain technology we use, they all have one thing in common.
Locking and securing non-modifiable information by signing specific data, with the imprinting process defined and specified by a policy.
To break this rigid structure and make this asset class more dynamic and useful, we need a common language or standard. The idea is to make NFTs readable and "executable" similar to small dynamic programs or microservices but immutable or more “decentralized”.

## Simple Summary
The basic idea is to attach reference scripts to NFTs that can be additionally interpreted and used by any instance that wants to provide a service for an iNFT. I call them i for intrinsic NFT or just smart/functional NFTs.

## Motivation

I will explain the idea and motivation behind this CIP in the following part.

With hundreds or thousands of NFT projects, it is unfortunate that we can only collect them. There is no possibility to do something else right now except for identity, social clubs, or lands. Just the ability to own something. Of course, this is already a big advantage. But integration with widely used services is yet to come.
More of a kind of usability needs to be integrated into the NFT body. This usability needs to be secure, useful and widely used by an open standard.

To make the process easier for everyone, we would need a different standard or interface that is easy to understand and use, based on our technical boundaries and general conditions.
There are many possibilities to integrate that functionality with the boundaries given by blockchain technology which makes it not that easy.

With the help of the community, I hope we can find a good solution.

The first question is.

Do we need that stuff at all?

At least I can say why I'm working on this idea. It would be very useful for the project I am currently working on, and I would say that there are a lot of people who would like to do more with their NFTs than just hold them.

The main question is:
How can we transform the NFT market in a way that users can use their assets like physical items e.g. credit cards, vouchers, etc. and how could it be integrated into existing software environments easily?
Not only the NFT market itself but also any other possible services for the web3 or interfaces between the real and the digital world.
We need a simple bridge, interface, or communication standard for the use of smart NFTs.

Signed digital objects with properties, values, and logical behavior. Native tokens are the basis of these NFTs. iNFTs (smart NFTs) consist of some kind of unique DNA. It would be best if there was a standard or interoperability with other DLT ecosystems.

Let's go deeper below.

## Transitions

This CIP provides a concept that expands the current CIP-0068 by adding executable reference scripts to NFTs.

### iNFT metadata standard

This proposal uses the idea behind CIP-0068, which defines the basis of on-chain minted NFTs via eUTxOs that can be minted and consumed by smart contracts.

To make the adoption and usability as easy as possible, we need to speak a common language that can be used by any party.

1. My first thoughts were to define an interpreter/compiler for running code off-chain, an NFT interpreter.
The code can be added to the metadata of the NFT or another reference input that contains the logic.

A definition like the following JSON. That code could be interpreted by anyone.

### Example ###
```
{
  "722": {
    "<policy_id>": {
      "<asset_name>": {
        "name": "<display_name>",
        "image": "<ipfs_link>",
        "mediaType": "<mime_type>",
        "description": "<description>",
        "files": [
          {
            "name": "<display_name>",
            "mediaType": "<mime_type_subfile_1>",
            "src": "<ipfs_link_subfile_1>"
          },
          {
            "name": "<display_name>",
            "mediaType": "<mime_type_subfile_2>",
            "src": "<ipfs_link_subfile_2>"
          }
        ],
        "dna": {
            "creator": "",
            "code": "<GUID>",
            "utilities": 
              "constants": [
                {
                  "name": "MIN_STAKE", // value checked through UTxO Datum given by registered Provider.
                  "value": "500"
                },
                {
                  "name": "DISCOUNT",
                  "value": "0.01"
                }
              ],
              "fetchMetaData": [
                {
                  "address": "<ANOTHER NFT ADDRESS OR TRANSACTION WHICH CONTAINS METADATA (must be written in 722 standard or TRANSACTION STANDARD (not defined yet))>",
                  "valueStore": "<VARIABLE NAME CONTAINING METADATA>"
                }
              ]
            },
            "genes": [
              {
                "id": "<GUID>",
                "name": "<NAME OF THE NFT PROPERTY # e.g. for (<LOTTERY | COUPON | LEGITIMACY | VALIDATOR)>",
                "description": "<OPTIONAL: SPECIFIC DESCRIPTION OF THE PROPERTY>",
                "definition": {
                  "inputs": [
                    {
                      "name": "<NAME OF INPUT PARAMETER>"
                    }
                  ],
                  "guards": [
                    [
                      {
                        "<NAME OF CONDITION e.g. checkMinStake>": "[OwnerWallet.StakeCount] > [[MIN_STAKE]]"
                      },
                      {
                        "<NAME OF CONDITION e.g. alreadyClaimed>": "[LatestReceipt.Slot] < General.CurrentSlot"
                      }
                    ]
                  ],
                  "tasks": [
                    {
                      "<NAME OF TASK e.g. multiplier>": "[OwnerWallet.LatestReward]*[[DISCOUNT]]"
                    }
                  ],
                  "properties": "<JOBJECT | ARRAY<JOBJECT>>"
                }
              }
            ],
            "DnaErrorValue": "0",
            "evolution": "1.0.0"
        }
      },
      "version": "1.0"
    }
  }
}
```

The names like DNA, or intrinsic values are just metaphors, that represent the “intelligent code” of an iNFT. But with this kind it makes it very complex, adding individual logic and not to mention interpreting it. To make it easier to define and read these programs there is an open-source project called Quicktype. https://app.quicktype.io/
It generates JSON data out of program code in any language like Haskell, c#, typescript, etc.

The code could then be added as JSON to the metadata (datum) and interpreted by anyone as a simple verified application.
This interpreter part and the use of different languages contribute to higher usability and better acceptance.

A downside of just writing the code as additional information is the interpretation. This would need trusted third parties or oracles running that code because it can't be executed on-chain.

It is also clear that any NFT logic or behavior has to be accepted by a/the service provider anyway, but running it in the chain would make it easier to integrate the standard since we could use the eUTxO model.

To add value/logical behavior to this kind of NFTs, we could also extend Plutus by a way to execute Haskell code that can be “injected” or interpret by a datum, for example. But that would open a lot of new security holes that would have to be evaluated carefully.

As an idea, this kind of code could be executed only if the consuming NFT/datum is from the owner of the Plutus script itself by making a wallet/policy owner check mandatory. I mention this only as an alternative way.

Nevertheless, the following idea came ito my mind. True on-chain Smart NFTs where the Plutus core does not need to be updated. We use the recently added mechanism of reference scripts.

## Abstract

### iNFTs via Reference Scripts
Think of smart NFTs as unique objects with a logical code base that can be used to interact with on-chain - that code is itself a smart contract.
Let's go more in detail a little bit.

### Overview

![Alt text](cip_overview.png?raw=true "CIP-0078 overview")
## Explanation

The standard defines an common interface for iNFTs. The following is a first idea with basic functions, that every iNFT needs to implement. Each single function will be explained more in detail below.

### Interface standard

#### Update
The update endpoint is a modifier that updates the Properties datum. Its implementation must take into account that only an acceptor can use this endpoint.

#### Execute
The execute endpoint is the individual “DNA” of an NFT. It is up to the creator what the NFT is made for. e.g. an NFT could generate tokens or trigger something. Maybe this part does not make any sense yet.

#### Lock
The lock endpoint is the part where any user could lock some UTxOs, which can only be claimed by the owner and its specific validator script conditions.

#### Claim
The claim endpoint is there to consume UTxOs.

#### RegisterAcceptor
The register endpoint is the part where any user can add their service to the NFT. By registering with the NFT, you should accept the NFT conditions on your service side. The owner of this NFT should be able to get the given advantages made by the NFT definition.

#### DeregisterAcceptor
The endpoint to deregister a service partner.

### Datums
The standard extends the existing CIP-0068 by adding reference scripts to NFTs. These kinds of NFTs have multiple datums that are defined below.
Some ideas behind the separation and information was taken as an inspiration from https://medium.com/mysten-labs/sui-dynamic-nfts-to-enhance-gameplay-47ee79bbb89c

#### Description
This part of the NFT reference script contains the overall description with some kind of metadata information.

#### Stats
The stats are the immutable part of the NFT. Nobody can change the stats of this NFT. Stats are the value and can be consumed by the smart contract or a third-party project.

#### Properties
The properties are modifiable values that are part of changeable NFTs like they exist in gaming for example. The property should only be modifiable by the acceptor or service provider.

#### Condition
Conditions are specific rules for the validation script, that is run by the claim and/or execute function. Conditions are also immutable.

#### History
History is a kind of log datum, that collects arbitrary information during its existence.

TODO:
Defining and describing the endpoints and datums in more detail.

## Rationale

To be updated

## Copyright

This CIP is licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode)

