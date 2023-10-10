# good-practice-shipment-tracking
This repository contains the good practice to implement shipment tracking with ONE Record

- [ONE Record-based shipment tracking](#one-record-based-shipment-tracking)
  * [Basic Information on this document](#basic-information-on-this-document)
    + [Objective](#objective)
    + [Target audience](#target-audience)
    + [Geographical coverage](#geographical-coverage)
    + [Creators](#creators)
    + [Continous development and availability](#continous-development-and-availability)
    + [Use and reference](#use-and-reference)
    + [Publication date, version and history](#publication-date--version-and-history)
  * [Dependencies](#dependencies)
    + [Standards applied](#standards-applied)
    + [ONE Record Server Implementation used](#one-record-server-implementation-used)
  * [Assumptions](#assumptions)
  * [Solution approach](#solution-approach)
  * [Piece-centricity and physics-orientation](#piece-centricity-and-physics-orientation)
- [Data use](#data-use)
  * [Request design](#request-design)
  * [Response](#response)
    + [Header](#header)
    + [AWB data](#awb-data)
    + [Shipment data](#shipment-data)
    + [Piece linked to the shipment](#piece-linked-to-the-shipment)
    + [TransportMovement of the piece](#transportmovement-of-the-piece)
    + [Events](#events)
      - [MAN](#man)
      - [RCF](#rcf)
  * [Special Case: Multi-Carrier tracking platform](#special-case--multi-carrier-tracking-platform)
- [API application](#api-application)
  * [Required functions](#required-functions)
- [Authentication approach](#authentication-approach)
- [Effective date](#effective-date)

## Basic Information on this document

### Objective 

In 2022, major stakeholders of the supply chain decided to aim for a renewed data sharing infrastructure for the supply chain by 2026. This process was initiated and is moderated by the International Air Transportation Association (IATA). As a first step towards a holistics ONE Record-based eco-system, some parties agreed to implement an open shipment tracking API in 2023 (exact date tbd.). Thus, the purpose of this document is to provide a Good Practice for a shipment tracking API in the IATA ONE Record-based data eco-system for the supply chain. 

The open tracking use case is not limited to carriers. As described below, there is also a place for data platform, shippers and many other stakeholders to apply this use case.

### Target audience
This document can be used by any party with the interest in the topic. 

"Shipment" is defined as pieces under one contract, and not limited to the Master- or House-AWB. As ONE Record aims for multi-modality, this document should also find use in other modes of transportation beyond air freight only.

### Geographical coverage
As there are no legal or operational restrictions, the solution can be used world wide.

### Creators
This document is the outcome of the work of the "Joint ONE Record piloting and transition working group // technical part" at IATA. It was orchestrated by Arnaud Lambert of IATA as Secretary, with Philipp Billion of Lufthansa Cargo as Chairman. 

Major contributions were made by:

* Lufthansa Cargo, Dr. Philipp Billion
* Lufthansa Industry Solutions, Dr. Daniel A. Döppner
* Air Canada, Josh Priebe
* Riege Software, Martin Skopp
* IATA, Arnaud Lambert
* DHL, Mary Stradling
* Air France / KLM, Bilel Chakroun
* GLS HKG, Keith Lam
* Nexshore Technologies, Pramod Rao
* British Telecom, Mark Belliss
* Qatar Airways, Ajay Manoharan
* Colog AG, Matthias Hurst
* MDF Solutions, Martin Fowler

A special thanks to Niclas Scheiber, Frankfurt University of Applied Sciences for preparing version 3.0 of the business ontology of ONE Record in coordination with the IATA ONE Record data model focus group.

### Continous development and availability

This document is to be used and continously developed, even if the current major stakeholders should move to other topics. Thus a "handover" of this document in Github is planned if responsibilities should shift.

### Use and reference

This Good Practice is free to access and use. If you use it, please refer to this document explicitly plus provide a link to the Github repository as source. This will ensure know-how-transfer and transparency.

### Publication date, version and history

Publication date, version and history should be provided by the Github version control system and not be duplicated here.

## Dependencies

### Standards applied

The implementation of this business case is completely based on the IATA ONE Record standard: [Github Reposity for ONE Record](https://github.com/IATA-Cargo/ONE-Record).

The ONE Record business ontology version 3.0 as of #release_date was used [Working draft Ontology of v3.0](https://github.com/NiclasScheiber/1R-DM-working-files/tree/main/working).

The ONE Record API and security specification draft witout a version as of #tobeUpdated APR 13, 2022 was used (no link available yet).

### ONE Record Server Implementation used

Principally, this approach is server software agnostic. Any server software can be used that communicates according to the requirements of ONE Record. Still there are some open source implementations available. 

[Overview of software tools related to ONE Record by Daniel Döppner](https://github.com/ddoeppner/awesome-one-record)

## Assumptions

Central assumptions:

- One or more stakeholders of the supply chain are able to report progress in the transportation of shipments along the supply chain.

- One or more stakeholders are interested in retrieving this information. The party requesting this information has one or more uniqueIDs (usually HouseAWB numbers or a MasterAWB numbers) for the shipment.

- Both parties are using ONE Record as standard for information sharing.

## Solution approach

Baseline for this use case is sharing the same tracking information shared today by FSUs in cIMP/cXML-based messaging. Additional information beyond these standards can be shared (like the FIW/FOW milestones that cannot be shared in cIMP). 

The solution should principally "open" to maximize the user´s benefits and minimize hurdles of implementation. This means that there a basic layer of information should be available for data consumers without authentication. Some stakeholders might still require technical features like API keys for technical management, hurdles should be kept as low as possible for the user.

Although the base layer is as open as possible, additional, more sensitive information can be made available over the same API endpoints after an authentication. Thus, this use case is a good starting point for entering the ONE Record digital eco system.

## Piece-centricity and physics-orientation

Today, tracking information is usually provided on shipment level, but the ONE Record data model is piece-centric as a basic principle. As a second principle, ONE Record is oriented towards reflecting the physical world. Thus, in ONE Record, e.g. not an abstract legal object like the AWB reaches a the departure milestone, but an aircraft on a transport movement with pieces loaded onto it. Through inheritance, the link between pieces and transport movement can be done. When all pieces of a shipment have departed, one could say, that the shipment has departed. For more information on the ONE Record data model approach please refer to the [IATA ONE Record implementation playbook](https://www.iata.org/en/programs/cargo/e/one-record/).

This impacts the implementation in so far, as some milestones are linked to other objects in a ONE Record environment compared to the current legacy environment. 

The following table show the ONE Record reference and the legacy reference objects for the most common milestones:


| Milestone   | ONE Record reference object  | Legacy reference object  |  Comment |
|---|:-:|:-:|---|
| BKD: booked on a specific flight  |  BookingOption | AWB  |   |
| RCS: received from shipper or agent  |  Piece | AWB  |   |
| MAN: manifested on a specific flight |  Piece | AWB  |   |
| DEP: departed on a specific flight  | TransportMeans  | AWB  |   |
| RCF: received from a given flight | Piece  | AWB  |   |
| NFD: arrived at destination and the consignee or agent has been informed  |    Piece  |  AWB |   |
| CCD: cleared by Customs  | Piece  | AWB  |   |
| DLV: delivered to the consignee or agent |  Piece |  AWB |   |
| DIS: with a discrepancy  | Piece  | AWB  |   |
| FOH: Freight on Hand | Piece  | AWB / Piece Numbers  |   |
  
The impact of this "clean" approach is quite limited. The data consumer will still be able to make call on the preferred LogisticsObject, but get the results linked to the ONE Record reference object. The practical implementation is provided further down. 

# Data use

As an assumption, all data is provided by a single data owner in the following example. In a more sophisticated setting, carrier, GHA, and/or other parties might provide milestones along the supply chain. In that case, only links to external systems will be provided, and additional calls will be required by the data consumer to follow the linked data.

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
GET logistics-objects/awb-020-8377728 HTTP/1.1
Host: 1r.example.com
Accept: application/ld+json
```

Deviation from standard explained in detail:

|  Component | Explanation  | Example  | Example explanation   |
|---|:-:|:-:|---|
| Class indicator | Indicates the class of logistics objects for the following uniqueID  | awb   |  hard coded for the application in air cargo, could be different for other modes of transportation
| Separator | Separates the three components  | -  |  Separator between object name and awb prefix |
| AWB prefix | provides the AWB prefix as part of the uniqueID of the AWB | 020  |  Example of LH Cargo's prefix
| Separator | Separates the three components  | -  |  Separator between awb prefix and awb number |
| AWB number | provides the AWB number as part of the uniqueID of the AWB | 8337728  |  random example

## Example workflow

The following response serves as an example and will be explained step-by-step. 
It is a strongly simplified data set with a shipment with one piece only on one transport movement and two events:

Request:
```http
GET logistics-objects/awb-020-8377728 HTTP/1.1
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
   "https://onerecord.iata.org/Waybill#waybillNumber":"8377728",
   "https://onerecord.iata.org/Waybill#waybillPrefix":"020",
   "https://onerecord.iata.org/Waybill#carrierDeclarationSignature":"Max Smith"
   "https://onerecord.iata.org/Waybill#shipment": {
      "@type": "https://onerecord.iata.org/Shipment",
      "@id": "https://1r.example.com/logistics-objects/8a76ed85-959e-45d5-8c42-5fd39c08efb1"
   },
}
```
*This is a linked data version of the Waybill*

A client can also request an embedded version of the Waybil using the `embedded` query parameter, , e.g.
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
   "https://onerecord.iata.org/Waybill#waybillNumber":"8377728",
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
### Events

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

The mechanis as described above reflects the requirements of a single carrier or forwarder platform providing tracking data for themselfes or other stakeholders that are not using ONE Record. Beyond that, there is a scenario where a tracking platform might want to offer a unified tracking mechanism in a hetereogenous ONE Record / non ONE Record environment. All is covered by the mechanis as descibed above, except for the case the platform gets a call for an AWB number for a carrier that is providing data in ONE Record. 

A typical request in this case could look like this:

```http
GET /logistics-objects/awb-020-8377728 HTTP/1.1
Host: 1r.example.com
Accept: application/ld+json
```

Here, instead of providing the tracking data, the platform would need to re-direct the request to the airline´s ONE Record server. According to the HTTP standard, this could be done by answering with an HTTP/1.1 302 re-direct:

```http
HTTP/1.1 307 Temporary Redirect
Location: https://1r.carrier.com/logistics-objects/awb-020-837772
```

This mechanism enables e.g. a platform to provide a unified ONE Record tracking API in a hetereougenous environment, where single stakeholders are providing data in ONE Record format, and others don´t.


# API application

## Required functions
The open tracking use case is an easy starting point for a ONE Record transition. As it doesn`t directly include the conclusion of a contract and can usually be considered as "one way communication", not all technical ONE Record features must be used.

The following technical features are required on the data provider side:

- Implemented basic requests: GET, POST
- Generating and managing links for linked data
- Support publish and subscribe

Not neccessarily required are

- Processing external change requests
- Providing an audit trail
- Supporting access delegation

On the data consumer side, even less functions are required for pure data consumption from the open tracking API:

- Making basic GET request
- Retrieving data from linked data sources 
 

# Authentication approach

For this use case, the authentication approach and implementation is left over to the implementing party. 
As of the nature of the "open" tracking API, authentication might not be required at all.

# Effective date

The effective date of the commitment for implementing this Open Tracking API is to be defined.
