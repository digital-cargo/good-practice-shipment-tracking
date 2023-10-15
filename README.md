---
title: shipment-tracking
repository: https://github.com/digital-cargo/good-practice-shipment-tracking
version: 1.0.0
maintainer: Ingo Zeschky
deputy-maintainer: Daniel A. Doeppner
ontologies:
- https://onerecord.iata.org/ns/cargo/3.0.0
- https://onerecord.iata.org/ns/api/2.0.0
- https://onerecord.iata.org/ns/coreCodeLists/0.0.3
data-classes:
- https://onerecord.iata.org/ns/cargo#Waybill
- https://onerecord.iata.org/ns/cargo#Shipment
- https://onerecord.iata.org/ns/cargo#Piece
- https://onerecord.iata.org/ns/cargo#TransportMovement
- https://onerecord.iata.org/ns/cargo#Organization
- https://onerecord.iata.org/ns/cargo#LogisticsEvents
- https://onerecord.iata.org/ns/cargo#Location
- https://onerecord.iata.org/ns/cargo#Company
- https://onerecord.iata.org/ns/cargo#Carrier
- https://onerecord.iata.org/ns/cargo#Value
- https://onerecord.iata.org/ns/cargo#Loading
data-properties:
- https://onerecord.iata.org/ns/cargo#waybillNumber
- https://onerecord.iata.org/ns/cargo#waybillType
- https://onerecord.iata.org/ns/cargo#shipment
- https://onerecord.iata.org/ns/cargo#shipmentOfPieces
- https://onerecord.iata.org/ns/cargo#partofShipment
api-version:
- 2.0.0
api-endpoints:
- method: GET 
  path: /logistics-objects/{logisticsObjectId} 
- method: GET 
  path: /logistics-objects/{logisticsObjectId}
- method: GET 
  path: /logistics-objects/{logisticsObjectId}/logistics-events
- method: GET 
  path: /logistics-objects/{logisticsObjectId}/logistics-events/{logisticsEventId}
- method: POST 
  path: /notifications
- method: GET 
  path: /subscriptions
- method: POST 
  path: /subscriptions
- method: GET 
  path: /action-requests/{actionRequestId} 
- method: DELETE 
  path: /action-requests/{actionRequestId}
---

# Good Practice: Shipment Tracking

## Abstract

This good practice describes how shipment tracking data can be provided by one data owner and used by others in a simple and standardized way.
The audience are...
This document covers the relevant aspects of the ONE Record data model and the relevant ONE Record API interactions.
It is based on the ONE Record version 2.0.0 and the ONE Record data model 3.0.0.
It showcases how shipment tracking data can be provided by a data holder and consumed by others in a easiy to use and standardized way.


## Table of Contents

- [Good Practice: Shipment Tracking](#good-practice-shipment-tracking)
  - [Abstract](#abstract)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Basic Information on this document](#basic-information-on-this-document)
    - [Objective](#objective)
    - [Target audience](#target-audience)
    - [Geographical coverage](#geographical-coverage)
    - [Acknowledgements](#acknowledgements)
    - [Continuous development and availability](#continuous-development-and-availability)
    - [Use and reference](#use-and-reference)
    - [Publication date, version and history](#publication-date-version-and-history)
  - [Dependencies](#dependencies)
    - [Standards applied](#standards-applied)
    - [Other related use cases](#other-related-use-cases)
  - [Solution approach](#solution-approach)
  - [Piece-centricity and physics-orientation](#piece-centricity-and-physics-orientation)
  - [Data exchange scenarios](#data-exchange-scenarios)
    - [data holder depending on entry point](#data-holder-depending-on-entry-point)
      - [variant 1 - entry point ShipmentTracking - data shared only, i.e. not updated](#variant-1---entry-point-shipmenttracking---data-shared-only-ie-not-updated)
      - [variant 2 - ShipmentTracking based on ShipmentRecord](#variant-2---shipmenttracking-based-on-shipmentrecord)
      - [variant 3 - update of ShipmentTracking status, e.g. by GHA](#variant-3---update-of-shipmenttracking-status-eg-by-gha)
    - [subscription variants](#subscription-variants)
      - [subscription by party providing Data](#subscription-by-party-providing-data)
      - [subscription by other parties](#subscription-by-other-parties)
- [Data Model](#data-model)
- [Data provisioning](#data-provisioning)
  - [Data Model](#data-model-1)
  - [Data Mapping](#data-mapping)
- [Data consumption](#data-consumption)
  - [Describe different get approaches, get LO, get embedded LO, get LEs](#describe-different-get-approaches-get-lo-get-embedded-lo-get-les)
  - [Request design](#request-design)
  - [Example workflow](#example-workflow)
    - [Shipment data](#shipment-data)
    - [TransportMovement of the piece](#transportmovement-of-the-piece)
    - [Overview of LogisticsEvents](#overview-of-logisticsevents)
      - [MAN](#man)
      - [RCF](#rcf)
  - [Special Case: Multi-Carrier tracking platform in a heterogenous environment](#special-case-multi-carrier-tracking-platform-in-a-heterogenous-environment)
  - [Guidelines for implementation](#guidelines-for-implementation)
    - [Error Handling and ChangeRequest process](#error-handling-and-changerequest-process)
    - [Considerations / sharing experience for integration into internal IT landscape](#considerations--sharing-experience-for-integration-into-internal-it-landscape)
      - [Considerations for data mapping](#considerations-for-data-mapping)
      - [Considerations for Error Handling](#considerations-for-error-handling)
      - [Considerations for change and update process](#considerations-for-change-and-update-process)
- [API application](#api-application)
  - [Required functions](#required-functions)
  - [Authentication approach](#authentication-approach)
    - [Use case specific requirements for authentication](#use-case-specific-requirements-for-authentication)
  - [Useful references and assumptions](#useful-references-and-assumptions)
    - [Data model excerpt](#data-model-excerpt)
    - [Mapping 1R Data Model \<-\> Cargo IMP](#mapping-1r-data-model---cargo-imp)
    - [Diagrams with example data](#diagrams-with-example-data)
    - [Glossary](#glossary)
  - [References](#references)
    - [ONE Record Server Implementation used](#one-record-server-implementation-used)

--
## Introduction
Purpose of this Good Practice is to provide guidelines for exchanging Shipment Tracking related information using 
ONE Record standard.

The guidelines are based on 
- One Record API specification https://iata-cargo.github.io/ONE-Record/
- One Record cargo ontology 3.0
- Datamodel v3 RC5
- Cargo IMP Message Manual, 17th edition


## Basic Information on this document

### Objective
In 2022, major stakeholders of the supply chain decided to aim for a renewed data sharing infrastructure for the supply 
chain by 2026. This process was initiated and is moderated by the International Air Transportation Association (IATA). 
As a first step towards a holistics ONE Record-based eco-system, some parties agreed to implement an open shipment
 tracking API in 2023 (exact date tbd.). Thus, the purpose of this document is to provide a Good Practice for a shipment
 tracking API in the IATA ONE Record-based data eco-system for the supply chain.

The open tracking use case is not limited to carriers. As described below, there is also a place for data platform, 
shippers and many other stakeholders to apply this use case.

### Target audience
This document can be used by any party with the interest in the topic. 

"Shipment" is defined as pieces under one contract, and not limited to the Master- or House-AWB. 
As ONE Record aims for multi-modality, this document should also find use in other modes of transportation beyond 
air freight only.

### Geographical coverage
Since there are no legal or operational restrictions, the described solution can be used worldwide.

### Acknowledgements
The initial version of this document is the outcome of the 
"Joint ONE Record piloting and transition working group // technical part" at IATA. 
It was orchestrated by Arnaud Lambert of IATA as secretary and Philipp Billion of Lufthansa Cargo as chairman.

Major contributions were made by:

* Lufthansa Cargo, Dr. Philipp Billion
* Lufthansa Industry Solutions, Dr. Daniel A. Döppner
* Air Canada, Josh Priebe
* Riege Software, Martin Skopp
* IATA, Arnaud Lambert
* DHL, Mary Stradling
* Air France-KLM, Bilel Chakroun
* GLS HKG, Keith Lam
* Nexshore Technologies, Pramod Rao
* British Telecom, Mark Belliss
* Qatar Airways, Ajay Manoharan
* Colog AG, Matthias Hurst
* MDF Solutions, Martin Fowler

A special thanks to Niclas Scheiber, Frankfurt University of Applied Sciences for preparing version 3.0 of the 
business ontology of ONE Record in coordination with the IATA ONE Record data model focus group.

In Q3/Q4, 2023 updates to the initial version were elaborated to consider relevant changes and enhancements from data model v3.
Major contributors were a range of employees from LH Cargo IT and Fulfilment Digitization department in close co-operation with Niclas Scheiber.



### Continuous development and availability

This document is to be used and continuously developed, even if the current major stakeholders should move to other topics. 
Thus a "handover" of this document in Github is planned if responsibilities should shift.

### Use and reference

This Good Practice is free to access and use as we encourage its distribution and adoption in the industry. 
If you use it, please refer to this document explicitly plus provide a link to this Github repository as source. 
This ensures the transfer of know-how, transparency and scope of application in the industry.

### Publication date, version and history

Publication date, version and history should be provided by the Github version control system and not be duplicated here.


## Dependencies

### Standards applied

The implementation of this business case is completely based on the IATA ONE Record standard: 
[Github Reposity for ONE Record](https://github.com/IATA-Cargo/ONE-Record).

The ONE Record cargo ontology version 3.0 as of `2023-10-12` is used 
[Working draft Ontology of v3.0](https://github.com/NiclasScheiber/1R-DM-working-files/tree/main/working).

The ONE Record API and security specification draft without a version as of #tobeUpdated APR 13, 2022 was used (no link available yet).

### Other related use cases

The use case ShipmentTracking is closely related to use case Shipment Record used to exchange shipment data, e.g. 
derived from (M)AWB and HAWB data. A range of data elements shared as part of this use case are a part of 
ShipmentRecord related data. This also has an impact on the data exchange scenarios described later in this document.


## Solution approach

Baseline for this use case is sharing the same tracking information shared today by FSUs in cIMP/cXML-based messaging. 
Additional information beyond these standards can be shared (like the FIW/FOW milestones that cannot be shared in cIMP). 

The solution should principally "open" to maximize the user's benefits and minimize hurdles of implementation. 
This means that a basic layer of information should be available for data consumers without authentication. 
Some stakeholders might still require technical features like API keys for technical management, 
hurdles should be kept as low as possible for the user.

Although the base layer is as open as possible, additional, more sensitive information can be made available over the same API endpoints after an authentication. Thus, this use case is a good starting point for entering the ONE Record digital eco system.

## Piece-centricity and physics-orientation

Today, tracking information is usually provided on shipment level, but the ONE Record data model follows piece-centricicity as a core design principle. 
As a second principle, ONE Record is oriented towards reflecting the physical world. 
Thus, in ONE Record, e.g. not an abstract legal object like the AWB reaches a shipment tracking milestone, 
but an aircraft on a transport movement with pieces loaded onto it. 
Through inheritance, the link between pieces and transport movement can be done. 
When all pieces of a shipment have departed, one could say, that the shipment has departed. 
For more information on the ONE Record data model approach please refer to the 
[IATA ONE Record implementation playbook](https://www.iata.org/en/programs/cargo/e/one-record/).

This impacts the implementation in so far, as some milestones are linked to other objects in a ONE Record environment compared to the current legacy environment.

The following table shows the ONE Record reference and the legacy reference objects for the milestones available in legacy Cargo IMP and Cargo XML:


| Milestone / FSU code, description                                                  | ONE Record reference object       | Legacy reference object      | Comment |
| ---------------------------------------------------------------------------------- | --------------------------------- | ---------------------------- | ------- |
| BKD: booked on a specific flight                                                   | Piece / Shipment                  | AWB / Piece Numbers          |         |
| FIW: Freight Into Warehouse Control                                                | Piece / Shipment                  | AWB / Piece Numbers          |         |
| FOW: Freight Out of Warehouse Control                                              | Piece / Shipment                  | AWB / Piece Numbers          |         |
| DOC: Documents received by Handling Party                                          | Piece / Shipment                  | AWB / Piece Numbers          |         |
| FOH: Freight on Hand                                                               | Piece / Shipment                  | AWB / Piece Numbers          |         |
| RCS: received from shipper or agent                                                | Piece / Shipment                  | AWB / Piece Numbers          |         |
| PRE: prepared for loading on a specific flight                                     | Piece / Shipment / TransportMeans | AWB / Piece Numbers / Flight |         |
| MAN: manifested on a specific flight                                               | Piece / Shipment / TransportMeans | AWB / Piece Numbers / Flight |         |
| DEP: departed on a specific flight                                                 | Piece / Shipment / TransportMeans | AWB / Piece Numbers / Flight |         |
| ARR: arrived on a specific flight                                                  | Piece / Shipment / TransportMeans | AWB / Piece Numbers / Flight |         |
| RCF: received from a given flight                                                  | Piece / Shipment / TransportMeans | AWB / Piece Numbers / Flight |         |
| AWR: documents arrived on a given flight or surface transport of the given airline | Piece / Shipment / TransportMeans | AWB / Piece Numbers / Flight |         |
| NFD: arrived at destination and the consignee or agent has been informed           | Piece / Shipment                  | AWB / Piece Numbers          |         |
| AWD: arrival documents have been delivered to the consignee or his agent           | Piece / Shipment                  | AWB / Piece Numbers          |         |
| DLV: delivered to the consignee or agent                                           | Piece / Shipment                  | AWB / Piece Numbers          |         |
| DDL: delivered to consignee door                                                   | Piece / Shipment                  | AWB / Piece Numbers          |         |
| TRM: to be transferred to another airline                                          | Piece / Shipment / TransportMeans | AWB / Piece Numbers / Flight |         |
| TFD: transferred to another airline                                                | Piece / Shipment / TransportMeans | AWB / Piece Numbers / Flight |         |
| TGC: transferred to Customs/Government control                                     | Piece / Shipment                  | AWB / Piece Numbers          |         |
| RCT: received from another airline                                                 | Piece / Shipment / TransportMeans | AWB / Piece Numbers / Flight |         |
| CCD: cleared by customs                                                            | Piece / Shipment                  | AWB / Piece Numbers          |         |
| CRC: reported to customs                                                           | Piece / Shipment / TransportMeans | AWB / Piece Numbers / Flight |         |
| DIS: with a discrepancy                                                            | Piece / Shipment / TransportMeans | AWB / Piece Numbers / Flight |         |

In additon, the following disprecancy codes should be considered:

The impact of this "clean" approach is quite limited. 
The data consumer will still be able to make call on the preferred Logistics Object, 
but get the results linked to the ONE Record reference object. 
The appraoch to request data is described below. 

## Data exchange scenarios

General assumptions for data exchange:
- One or more stakeholders of the supply chain are able to report progress in the transportation of shipments along 
the supply chain.
- One or more stakeholders are interested in retrieving this information. The party requesting this information has 
one or more unique IDs (usually HouseAWB numbers or a MasterAWB numbers) for the shipment.
- Both parties are using ONE Record as standard for information sharing.

### data holder depending on entry point

1) Entrypoint is Waybill data object, data consumer knowns AWB number, e.g. 020-12345676. Data consumer can follow property #shipment in Waybill to get Shipment data and - if available - LogisticsEvents for a Shipment. 
1-1) 
1-2) 

#### variant 1 - entry point ShipmentTracking - data shared only, i.e. not updated
In this variant it is assumed that OneRecord related data exchange is limited of sharing shipment status information with other parties. 
Logistics objects then just need to be created and updated by the party sharing the data which is usually the operating carrier.
Since any updates to data are triggered by the same party the 1R ChangeRequest process does not apply in this case.
 
#### variant 2 - ShipmentTracking based on ShipmentRecord
Assumption in this variant is that a forwarder, shipper or other party also uses 1R to share AWB/HAWB data via 1R ShipmentRecord use case.
Some logistics objects used for use case ShipmentRecord are also used for use case ShipmentTracking, e.g. quantity details of the shipment.
In this case 1R Change Request process might have to be applied, i.e. whenever an existing logistics object is updated by a different party who is not the owner of the data. Example:
- forwarder provides AWB data via ShipmentRecord use case
- carrier shares ShipmentTracking data where pieces and/or weight has changed
=> carrier then has to request update to pieces/weight with forwarder using 1R CR process

#### variant 3 - update of ShipmentTracking status, e.g. by GHA
ShipmentTracking information also can be used by a party to trigger update of a shipment status and related shipment data at a different party.
This applies e.g. when a GHA shares ShipmentTracking information with a carrier he is handling.
Similarly to variant 2, updates to logistics object owned by a different party are subject to 1R Change Request process.


### subscription variants
ShipmentTracking information can be shared with partners in two ways, either by pro-actively subscribing a known business partner or by request of a partner or 3rd party.

####  subscription by party providing Data
For traditional FSU messaging transfer usually the relevant business partner, i.e. forwarding agent who has issued the AWB, is notified by the carrier proactively. This party usually also performs the booking and provides shipment data beforehand. In this context e.g. a completed booking and space allocation trigger submitting FSU messages to the business partner. For this purpose the carrier might maintain the messaging address (e.g. PIMA or SITA Address) in the internal customer database or any equivalent system.
A similar process is supported by One Record, i.e. notification process to a known business partner can be triggered by a dedicated shipment status, e.g. Booking completed - BKD. When e.g. BookingData and/or ShipmentRecord related data is shared via 1R beforehand this might be used as trigger.
Instead of a messaging address the OneRecord Server URI associated with that business partner is used to pass information to the right party. Similar to traditional messaging the carrier might have to maintain a list of URIs and related business partners for this purpose. 

####  subscription by other parties
In addition to a known business partner other parties might be interested in the shipment status, e.g. Broker, Consignee, GHA, etc. These parties can actively subscribe by addressing a subscription request for a unique shipment ID such as an Air Waybill number.
1R Server will then continue sharing information with that parties whenever shipment status information for the related id is updated.

# Data Model

**Conceptual Model**

**Class Diagam**
```mermaid
class Waybill
class Shipment
class Piece
class TansportMovement
class 

```
(for further information, see [ONE Record Cargo Ontology](https://onerecord.iata.org/ns/cargo))

# Data provisioning

## Data Model

## Data Mapping


# Data consumption

## Describe different get approaches, get LO, get embedded LO, get LEs 

For the sake of better comprehensibility, in the following examples it is assumed tht all different data objects are 
provided by a single data owner and hosted on a single ONE Record server.
In a more realistic environment, data objects are distributed across multiple ONE Record servers. These can be, 
for example, carriers, ground handling agent (GHA), and other parties that also provide milestones and status updates
along the supply chain.
In that case, only links to external systems will be provided, and additional calls will be required by the data 
consumer to follow the linked data principle.

## Request design

The basic structure of ONE Record URIs is quite open. Examples can be found in the current API specification draft (https://ddoeppner.github.io/ONE-Record/).

Examples of a basic URI for a logistics object: 

```
https://1r.example.com/logistics-objects/flight-LH870-2023-11-20
```

or

```
https://1r.example.com/logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c
```

Example of a URI for all logistics events linked with the logistics object: 

```
https://1r.example.com/logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c/logistics-events
```

Example of a URI for a specific event linked with the logistics object: 

```
https://1r.example.com/logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c/logistics-events/f2512fd
```

The URI can either be tokenized like

```
https://1r.example.com/logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c
```

or using class names like

```
https://1r.example.com/logistics-objects/transport-means-D-ALFA

```

as a basic structure. 

Both approaches are compliant with the current ONE Record specification. For tokenized URIs, it is assumed that there is always a first contact between data provider and data consumer in a ONE Record world. This results in a specific problem for the given use case. For an "open" API, a previous contact with a subscription cannot be assumed. For the "first contact", the data consumer requires the unique tokenized ID for a shipment to request the data from the data owner. However, at this point the tokenized URI is not yet known or communicated. The data provider on the other side cannot provide the tokenized URI because the data consumer is unknown due to the assumption of an "open API".
 
To solve this problem, for this specific use case, the URI for the GET request should contain the AWB number as the uniqueID for the request. 
The following section describes a reference implementation.

```http
GET logistics-objects/awb-020-12345675 HTTP/1.1
Host: 1r.example.com
Accept: application/ld+json
```

Deviation from standard explained in detail:

| Component       |                             Explanation                             | Example  | Example explanation                                                                               |
| --------------- | :-----------------------------------------------------------------: | :------: | ------------------------------------------------------------------------------------------------- |
| Class indicator | Indicates the class of logistics objects for the following uniqueID |   awb    | hard coded for the application in air cargo, could be different for other modes of transportation |
| Separator       |                   Separates the three components                    |    -     | Separator between object name and awb prefix                                                      |
| AWB prefix      |     provides the AWB prefix as part of the uniqueID of the AWB      |   020    | Example of LH Cargo's prefix                                                                      |
| Separator       |                   Separates the three components                    |    -     | Separator between awb prefix and awb number                                                       |
| AWB number      |     provides the AWB number as part of the uniqueID of the AWB      | 12345675 | random example                                                                                    |

## Example workflow

The following response serves as an example and will be explained step-by-step.
It is a strongly simplified data set with a shipment with one piece only on one transport movement and two events:

Request:
```http
GET logistics-objects/awb-020-12345675 HTTP/1.1
Host: 1r.example.com
Accept: application/ld+json
```

Response:

```http
HTTP/1.1 307 Temporary Redirect
Location: https://1r.example.com/logistics-object/1a8ded38-1804-467c-a369-81a411416b7c
```

The response contains the actual location of the requested object using the Location http header. 
The client then has retrieved the unique tokenized ID of the Waybill and can get them by requesting the actual URI.

Request:

```http
GET logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c HTTP/1.1
Host: 1r.example.com
Accept: application/ld+json
```

Response:

```bash
HTTP/1.1 200 OK
Content-Type: application/ld+json
Content-Language: en-US
Location: https://1r.example.com/logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c
Type: https://onerecord.iata.org/Waybill
Revision: 1
Latest-Revision: 1

{
   "@id":"https://1r.example.com/logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c",
   "@type":[
      "https://onerecord.iata.org/Waybill",
      "https://onerecord.iata.org/LogisticsObject"
   ],
   "https://onerecord.iata.org/Waybill#waybillType":"Master",
   "https://onerecord.iata.org/Waybill#carrierDeclarationDate": {
      "@type":"http://www.w3.org/2001/XMLSchema#dateTime",
      "@value":"2022-12-01T00:00:00Z"
   },
   "https://onerecord.iata.org/Waybill#waybillNumber":"12345675",
   "https://onerecord.iata.org/Waybill#waybillPrefix":"020",
   "https://onerecord.iata.org/Waybill#carrierDeclarationSignature":"Max Smith"
   "https://onerecord.iata.org/Waybill#shipment": {
      "@type": "https://onerecord.iata.org/Shipment",
      "@id": "https://1r.example.com/logistics-objects/8a76ed85-959e-45d5-8c42-5fd39c08efb1"
   },
}
```
*This is a linked data version of the Waybill*

A client can also request an embedded version of the Waybill using the `embedded` query parameter, , e.g.
```http
GET logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c?embedded=true HTTP/1.1
Host: 1r.example.com
Accept: application/ld+json
```

Two important remarks on this:

1. Only data hosted on the same ONE Record server can be embedded, as otherwise the ownership control of another party would be violated.
2. Even if data is embedded, a link for every logistics object must be created additionally, to enable essential ONE Record features like audit trail, pub/sub, access control, etc. for these objects.


### Shipment data

The data field *Waybill#shipment* contains a link to a shipment. 
A shipment in ONE Record is the totality of physical entities under one contract. 
The *Shipment#totalGrossWeight* is a typical data field belonging in this object. 

```json
{
   "@id":"https://1r.example.com/logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c",
   "@type":[
      "https://onerecord.iata.org/Waybill",
      "https://onerecord.iata.org/LogisticsObject"
   ],
   "https://onerecord.iata.org/Waybill#waybillType":"Master",
   "https://onerecord.iata.org/Waybill#carrierDeclarationDate": {
      "@type":"http://www.w3.org/2001/XMLSchema#dateTime",
      "@value":"2022-12-01T00:00:00Z"
   },
   "https://onerecord.iata.org/Waybill#waybillNumber":"12345675",
   "https://onerecord.iata.org/Waybill#waybillPrefix":"020",
   "https://onerecord.iata.org/Waybill#carrierDeclarationSignature":"Max Smith"
   "https://onerecord.iata.org/Waybill#shipment": {
      "@type": "https://onerecord.iata.org/Shipment",
      "@id": "https://1r.example.com/logistics-objects/8a76ed85-959e-45d5-8c42-5fd39c08efb1"
   },
}
```

```
### Piece linked to the shipment

Here, the piece has volume, dimensions and special handling codes (GEN, SPX and EAP).


```json
"https://onerecord.iata.org/Shipment#containedPieces": [
    {
   "@id": "https://1r.example.com/logistics-objects/
   		los/d94de4ba?embedded=true",
    "@type": [
        "https://onerecord.iata.org/Piece",
        "https://onerecord.iata.org/LogisticsObject"],
    "https://onerecord.iata.org/Piece#dimensions": {
        "@id": "_:1912794519",
        "@type": ["https://onerecord.iata.org/Dimensions"],
        "https://onerecord.iata.org/Dimensions#volume": {
            "@id": "_:620425442",
            "@type": [
                "https://onerecord.iata.org/Value"],
                "https://onerecord.iata.org/Value#value": 0.22361,
                "https://onerecord.iata.org/Value#unit": "MTQ"
                },
            "https://onerecord.iata.org/Dimensions#height": {
                "@id": "_:1880868295",
                "@type": ["https://onerecord.iata.org/Value"],
                "https://onerecord.iata.org/Value#value": 88.0,
                "https://onerecord.iata.org/Value#unit": "CMT"
                },
            "https://onerecord.iata.org/Dimensions#length": {
                "@id": "_:807894013",
                "@type": ["https://onerecord.iata.org/Value"],
                "https://onerecord.iata.org/Value#value": 33.0,
                "https://onerecord.iata.org/Value#unit": "CMT"
                },
            "https://onerecord.iata.org/Dimensions#width": {
                "@id": "_:1752661327",
                "@type": ["https://onerecord.iata.org/Value"],
                "https://onerecord.iata.org/Value#value": 77.0,
                "https://onerecord.iata.org/Value#unit": "CMT"
                }
            },
        "https://onerecord.iata.org/Piece#handlingInstructions": [
            {
            "@id": "_:36393723",
            "@type": ["https://onerecord.iata.org/HandlingInstructions"],
            "https://onerecord.iata.org/HandlingInstructions#serviceType": 
				"SPH",
            "https://onerecord.iata.org/HandlingInstructions#serviceDescription": 
            	"E-FREIGHT CONSIGNMENT WITH ACCOMPANYING DOCUMENTS",
            "https://onerecord.iata.org/HandlingInstructions#serviceTypeCode": 
            	"EAP"
             }
        ]
                
```
### TransportMovement of the piece

The linked TransportMovement only contains origin and destination of the leg:

```json
"https://onerecord.iata.org/Piece#transportMovements": [
 	{
 	"@id": "https://1r.example.com/logistics-objects/los/
 		170fcd042a894b16b8d85d14916b7619?embedded=true",
	"@type": [
		"https://onerecord.iata.org/TransportMovement",
       "https://onerecord.iata.org/LogisticsObject"
       	],
    "https://onerecord.iata.org/TransportMovement#departureLocation": {
    	"@id": "_:1093851795",
      	"@type": ["https://onerecord.iata.org/Location"],
		"https://onerecord.iata.org/Location#code": "FRA"
		},
	"https://onerecord.iata.org/TransportMovement#arrivalLocation": {
		"@id": "_:956103849",
		"@type": ["https://onerecord.iata.org/Location"],
		"https://onerecord.iata.org/Location#code": "JFK"
		}
```
### Overview of LogisticsEvents

Like any other objects, events have links and are linked, and thus are used in the following way:

#### MAN

```json
"https://onerecord.iata.org/LogisticsObject#events": [
{
	"@id": "https://1r.example.com/logistics-objects/los/
		/070bfcc011194fb2b54d181067e875e7",
	"@type": ["https://onerecord.iata.org/Event"],
	"https://onerecord.iata.org/Event#eventName": "Consignment manifested on a specific flight",
	"https://onerecord.iata.org/Event#location": {
		"@id": "_:896881601",
		"@type": ["https://onerecord.iata.org/Location"],
			"https://onerecord.iata.org/Location#code": "FRA"
		},
	"https://onerecord.iata.org/Event#eventCode": "MAN",
	"https://onerecord.iata.org/Event#eventTypeIndicator": "actual",
	"https://onerecord.iata.org/Event#linkedObject": {
		"@id": "https://1r.example.com/logistics-objects/los/
			170fcd042a894b16b8d85d14916b7619?embedded=true",
		"@type": [
			"https://onerecord.iata.org/TransportMovement",
			"https://onerecord.iata.org/LogisticsObject"]
		},	
	"https://onerecord.iata.org/Event#dateTime": {
		"@type": "http://www.w3.org/2001/XMLSchema#dateTime",
		"@value": "2022-12-01T15:03:00Z"
       }
  }
                       
                          
```

#### RCF

```json
{
	"@id": "https://1r.example.com/logistics-objects/los/
		070bfcc011fsa4fb2b54d181067e875e7",
	"@type": ["https://onerecord.iata.org/Event"],
	"https://onerecord.iata.org/Event#eventName": "Consignment received from a given flight",
	"https://onerecord.iata.org/Event#location": {
		"@id": "_:1382884449",
		"@type": ["https://onerecord.iata.org/Location"],
		"https://onerecord.iata.org/Location#code": "JFK"
			},
	"https://onerecord.iata.org/Event#eventCode": "RCF",
	"https://onerecord.iata.org/Event#eventTypeIndicator": "Scheduled",
	"https://onerecord.iata.org/Event#linkedObject": {
		"@id": "https://1r.example.com/logistics-objects/los/
			d94de4ba?embedded=true",
		"@type": [
			"https://onerecord.iata.org/Piece",
			"https://onerecord.iata.org/LogisticsObject"]
		},
	"https://onerecord.iata.org/Event#dateTime": {
		"@type": "http://www.w3.org/2001/XMLSchema#dateTime",
		"@value": "2022-12-02T11:20:00Z"
		}
	}
```

## Special Case: Multi-Carrier tracking platform in a heterogenous environment

The mechanism as described above reflects the requirements of a single carrier or forwarder platform providing tracking data for themselves or other stakeholders that are not using ONE Record. Beyond that, there is a scenario where a tracking platform might want to offer a unified tracking mechanism in a heterogenous ONE Record / non ONE Record environment. All is covered by the mechanism as described above, except for the case the platform gets a call for an AWB number for a carrier that is providing data in ONE Record. 

A typical request in this case could look like this:

```http
GET /logistics-objects/awb-020-1234575 HTTP/1.1
Host: 1r.example.com
Accept: application/ld+json
```

Here, instead of providing the tracking data, the platform would need to re-direct the request to the airline´s ONE Record server. According to the HTTP standard, this could be done by answering with an HTTP/1.1 302 re-direct:

```http
HTTP/1.1 307 Temporary Redirect
Location: https://1r.carrier.com/logistics-objects/awb-020-1234575
```

This mechanism enables e.g. a platform to provide a unified ONE Record tracking API in a heterogenous environment, where single stakeholders are providing data in ONE Record format, and others don't.
  


## Guidelines for implementation

### Error Handling and ChangeRequest process
If applicable, 1R ChangeRequest process has to be applied when updating logistics objects for a certain shipment.
Relevance of 1R ChangeRequest process depends on the applied data exchange scenario. Please refer to details above.
For any logistics object related updates where equivalent data elements exist in traditional messaging specifications 
it is recommended of using the appropriate MIP "Error" Code along with ChangeIndicator "C" as specified in IATA Message 
Improvement Programme as reference.

When data is updated, shared or requested to be shared errors might occur, e.g. requested shipment id is unknown, server 
not available, logistics object update restricted due to different owner, etc. In general, the standard processes as specified in IATA 1R documentation apply.
For any logistics object related errors where equivalent data elements exist in traditional messaging specifications it is recommended of using the appropriate MIP Error Code as specified in IATA Message Improvement Programme as reference.

The 1R Change Request process and Error Handling processed are specified in https://iata-cargo.github.io/ONE-Record, 
chapters Subscriptions, Notifications and Action Requests.


### Considerations / sharing experience for integration into internal IT landscape

#### Considerations for data mapping

Other than ONE Record, the data structure supported and used by a Transport Management System (TMS) and other applications 
involved in ShipmentTracking related data exchange might not (yet) support the piece centric concept. Moreover, 
there is usually no dedicated distinction between physical, contractual and other categories the data is related to. 

This also applies to traditional messaging standards such as IATA Cargo IMP, Cargo XML, company internal web services, etc.
Especially during the transition period it might therefore be required to convert data between 1R standard and other 
data formats and data structures.

For this purpose guidelines for mapping between the different data formats must be defined and applied.
Apart from the need of defining one dimensional mapping rules between the concerned data elements, general directions 
of how to organize transferring data between those different structures must be defined.

This also applies to following areas of the ShipmentTracking use case:
- modelling and usage of classes and data elements from 1R data model
- shipment status (also referred to as milestone or event) information - mapping on shipment level only, on piece level only or on both levels
- mapping of information for split shipments, i.e. shipments handled or moved in different parts

- concepts to map planning and actual status information
  - info from BKD status / booking info to be used for planned milestones
  - info from other status codes to be used for actual milestones

- transport mode specific conventions for certain data elements
  - CarrierCode, Flight Number and Departure Day/Month in data element <transportIdentifier>
  - Status Event Code, Reason Code (for DIS) and Partial ID (shipment level only) in data element <eventCode>
	 
```json
{
    "@context": {
        "vocab": "https://onerecord.iata.org/ns/cargo#"
    },
    "@type": "LogisticsEvent",
    "@id": "https://1r.example.com/logistics-object/abcd/logistics-events/xyz",
    "eventCode": {
        "@id": " https://onerecord.iata.org/ns/coreCodeLists#StatusCode_DEP"
    },
    "eventName": "Consignment departed on a specific flight",
    "eventDate": {
        "@type": "http://www.w3.org/2001/XMLSchema#dateTime",
        "@value": "2023-04-01T10:38:01.000Z"
    },
    "eventTimeType": {
        "@id": "https://onerecord.iata.org/ns/cargo#ACTUAL"
    },
    "recordedAtLocation": {
        "@type": "Location",
        "@id": "https://1r.example.com/logistics-objects/FRA",
        "locationCode": "FRA",
        "locationName": "Frankfurt Airport",
        "locationType": "Airport"
    }
}
```
([assets/logistics-event-DEP.json](./assets/logistics-event-DEP.json))
	 
#### Considerations for Error Handling
- MIP Codes 
- HTTP Status Codes
  - 401
  - 403
  - 404
  - 500

#### Considerations for change and update process 
- MIP Codes; C indicator; 
- Not relevant?


# API application

## Required functions

The open tracking use case is an easy starting point for a ONE Record transition. 
As it doesn't directly include the conclusion of a contract and can usually be considered as 
"one way communication", not all technical ONE Record features must be used.

The following technical features are required on the data provider side:

- Implemented basic requests: GET, POST
- Generating and managing links for linked data
- Support publish and subscribe

Not necessarily required are

- Processing external change requests
- Providing an audit trail
- Supporting access delegation

On the data consumer side, even less functions are required for pure data consumption from the open tracking API:

- Making basic GET request
- Retrieving data from linked data sources

## Authentication approach
- add Section about Security
  - Because of the specificaiton ofthe standard, every request to a logistics-object needs to be authenticated by definition.
  - The authoriation and access limitation is up to the implementer.

It is RECOMMENDED to restrict access to logistics objects and logistics events to keep track of data access and data consumers.

For this use case, the authorization approach is left over to the implementing party. 
As of the nature of the "open" tracking API, authentication might not be required at all.

### Use case specific requirements for authentication
- non sensitive vs. sensitive information
  sensitive data e.g.:
  - Tracking info for Valuable or Vulnerable shipments;
  - content of (M)AWB contractual data, i.e. beyond flight routing, quantity details and shipment status
- wer darf welche Daten sehen, etc. FWD, GHA, andere / Identifizierung


## Useful references and assumptions

### Data model excerpt
=> see reference

### Mapping 1R Data Model <-> Cargo IMP
As of now ShipmentTracking related data has been exchanged mostly via IATA Cargo IMP FSU and FSA messages. 
These messages provide status information for dedicated events of the airport to airport process on (M)AWB level. 
Same applied to equivalent IATA Cargo XML messages.
In contrast to that the 1R data model is based on piece level. Moreover, via 1R logistics event related information 
can be provided for any logistics objects available in the 1R data model. This involves differences to both, 
methodologies of data exchange and structure of data.
This has to be considered when migrating data exchange to 1R and/or transferring data between 1R and traditional 
data interchange methods.
The attached mapping instructions shall help to understand these differences, explain how to use the 1R data model 
to exchange ShipmentTracking related data, as well as provide guidelines of converting data from and to 1R. 
=> see reference

### Diagrams with example data
=> see reference



### Glossary
see [digita-cargo/glossary](https://github.com/digital-cargo/glossary)

## References

### ONE Record Server Implementation used

Principally, this approach is server software agnostic. Any server software can be used that communicates according to 
the requirements of ONE Record. Still there are some open source implementations available. 

[Overview of software tools related to ONE Record by Daniel Döppner](https://github.com/ddoeppner/awesome-one-record)