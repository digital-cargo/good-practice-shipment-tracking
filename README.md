<!--
---
title: shipment-tracking
repository: https://github.com/digital-cargo/good-practice-shipment-tracking
version: 1.0.0
maintainers: 
- Daniel A. Doeppner
- Ingo Zeschky
- Philipp Billion
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
- https://onerecord.iata.org/ns/cargo#actions
- https://onerecord.iata.org/ns/cargo#arrivalLocation
- https://onerecord.iata.org/ns/cargo#departureLocation
- https://onerecord.iata.org/ns/cargo#involvedInActions
- https://onerecord.iata.org/ns/cargo#shipment
- https://onerecord.iata.org/ns/cargo#partofShipment
- https://onerecord.iata.org/ns/cargo#transportIdentifier
- https://onerecord.iata.org/ns/cargo#loadedPieces
- https://onerecord.iata.org/ns/cargo#servedActivity
- https://onerecord.iata.org/ns/cargo#events
- https://onerecord.iata.org/ns/cargo#pieces
- https://onerecord.iata.org/ns/cargo#waybillNumber
- https://onerecord.iata.org/ns/cargo#waybillType
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
-->

# Good Practice: ShipmentTracking
[![Made with love for Digital Cargo](https://img.shields.io/badge/Made%20with%20%E2%9D%A4%20for-Digital%20Cargo-dce435)](https://digital-cargo.org)
[![GitHub](https://img.shields.io/github/license/digital-cargo/good-practice-shipment-tracking)](https://creativecommons.org/licenses/by/4.0/)
[![Releases](https://img.shields.io/github/v/release/digital-cargo/good-practice-shipment-tracking)](https://github.com/digital-cargo/good-practice-shipment-tracking/releases)

## Abstract

The logistics and cargo industry grapples with a prevalent and pressing issue: while APIs for shipment status data exchange exist, a glaring lack of standardization across them poses significant challenges. 
The consequence of this non-uniformity is evident: stakeholders are burdened with the expensive and time-consuming task of individualized integrations, harmonization of incompatible data formats from different sources, leading to inefficiencies, misunderstandings, and subsequent maintenance costs.

The ONE record standard remedies this situation.
By endorsing a shared data model and a unified API structure, it offers a streamlined platform for shipment status data exchange. 
This good practice document describes the methodology for providing tracking data and shipment status updates via ONE Record, making this data effortlessly accessible to others. 

Based on the ONE Record API version 2.0.0 and the ONE Record Data Model version 3.0.0, this document provides guidance on how to provide shipment tracking data in an easy-to-use and standardized manner.

## Introduction

In the dynamic world of logistics and cargo, shipment tracking stands as a cornerstone, ensuring visibility, predictability, and trust within the supply chain. 
Yet, as businesses expand and systems diversify, the industry faces a challenge: the myriad of non-standardized tracking systems, each requiring unique integration and understanding. 
This fragmentation not only complicates operations but also escalates costs and reduces efficiency.

Initiated and moderated by the International Air Transportation Association (IATA), in 2022, major stakeholders of the supply chain decided to aim for a renewed data sharing infrastructure for the global logistics networks by 2026.
Enter the ONE Record standard, which aims to unify, streamline and improve shipping data across the industry. 
By leveraging the ONE Record standard, stakeholders can draw on a unified data model and API that promotes seamless integration across various platforms and improves collaboration between various organizations. 
This standardization comes with a number of benefits, from reducing the complexity and cost of custom integrations to enhancing transparency and trust.
It lays the foundation for standardization, enabling a consistent data model and API across diverse platforms, thereby streamlining integrations and collaborations.
This uniformity heightens transparency, allowing stakeholders to effortlessly interpret shipment data, fostering trust throughout the supply chain.
Moreover, the standardized approach curtails complexities tied to integration, conserving both time and resources that might otherwise be diverted to bespoke solutions. 

As a first step, some parties agreed to implement a shipment tracking API in 2023 based on the novel ONE Record data standard.
The use case is not limited to carriers. As described below, there is also a place for data platform, shippers and many other stakeholders to apply this use case.

As a first step, some parties have agreed to introduce an API for shipment tracking based on the new ONE Record data standard in 2023.
The user group is not limited to freight forwarders; data platforms, shippers and many other players can also participate and use it.

Within the realm of shipment tracking data exchange, it's assumed that at least one stakeholder in the supply chain can report shipment progress, 
while others are keen on accessing this data using unique shipment IDs. And, that all of these parties agree on ONE Record as the standard for information exchange.

The purpose of this document is to explain how shipment tracking data exchange can be implemented using ONE Record.
Among other things, the goal is to highlight ONE Record's unique value proposition and motivate technical and business audiences to move to this standardized approach.
Readers will gain a comprehensive understanding of how ONE Record is revolutionizing shipment tracking, making it more efficient, accessible, and future-ready.

### Scope

This good practice details the application of the ONE Record standard specifically in the context of shipment tracking. 
By using this good practice, organizations can understand, adopt, and streamline their shipment tracking offering to global best practice.

**What this document covers:**

- **Business context**: Assumptions, prerequisites, and the broader business scenario where this good practice is applicable.
- **Technical examples**: Detailed descriptions and examples of the API calls, data model classes, data mappings, and their applications in the context of shipment tracking.
- **Transition recommendations**: Recommendations and guidelines that businesses should consider for a smooth and effective transition to ONE Record.

**What this document does not cover:**

- **Compelete implementations**: This good practice includes sample code to support knowledge transfer, it does not provide detailed implementation or out-of-the-box software.
- **Comparison with other standards**: This good practice describes the implementation with the ONE Record Standard. A comparison with other standards in the industry is not covered.
- **Vendor-specific implementations**: This document focuses on the standard itself and does not address specific third-party tools or solutions based on the ONE Record standard.
- **Complete technical specifications**: This document focuses solely on the ONE Record aspects pertinent to shipment tracking and doesn't encompass the entire technical breadth of the standard.
- **Industry-wide statistics**: This document does not provide exhaustive industry data or statistics on the adoption or performance of the ONE Record standard.

This guide is based on the published ONE Record specifications prevalent as of `2023-10-16`. 
As the industry evolves, it is imperative for stakeholders to keep up to date on subsequent versions or changes to the standard.

**Target audience**

This document is intended for anyone interested in this topic. 
Thus, `shipment` is defined as pieces under one contract and is not limited to the Air Waybill (AWB), which is used particularly in air freight. 
Since ONE Record aims at multimodality, this good practice should also be applicable to transport modes other than air transport.

**Geographical coverage**

This shipment tracking best practice is globally applicable, unhindered by regional or national distinctions. 
With no legal or operational barriers to its adoption, the outlined solution is primed for worldwide deployment. 
As a result, companies of any size, at any location, can take advantage of the standardized workflows and increased efficiencies created by ONE Record.

### Variants

#### One data holder shares data

In this variant it is assumed that ONE Record related data exchange is limited of sharing shipment status information with other parties. 
LogisticsObject and LogisticsEvent data objects are created and updated by the party sharing the data which is usually the operating carrier.
Since all updates to the data objects are triggered by the same party, the [ChangeRequest](https://onerecord.iata.org/ns/api#ChangeRequest) approval process is not required in this case.

#### One data holder shares and receives updates
 
Shipment Tracking information also can be used by a party to trigger update of a shipment status and related shipment data at a different party, e.g. by GHA
This applies e.g. when a GHA shares ShipmentTracking information with a carrier he is handling.
Updates to logistics object owned by a different party are subject to 1R Change Request process.

#### Multiple data holders share data and receive updates

In this variant, one party (e.g. a freight forwarder, shipper, or other party) uses ONE Record to publish LogisticsObjects data, and other parties 
link those LogisticsObjects, every party will also receive data updates from the others.

In this variant, the ONE Record Change Request process is used whenever an existing logistics object is updated by another party who is not the data holder.
For example, a forwarder publishes waybill, shipment and piece data objects and a carrier contributes shipment tracking data in the form of LogisticsEvents and/or corrects weights. 

As a consequence, the freight forwarder would have to request a change in the weights of the freight forwarder's packages via the [ONE Record ChangeRequest mechanism](https://iata-cargo.github.io/ONE-Record/logistics-objects/#update-a-logistics-object).

This variant is also called the [ShipmentRecord](https://github.com/digital-cargo/good-practice-shipment-record) exchange use case.
Therefore, some logistics objects used for use case ShipmentRecord are also used for use case ShipmentTracking, e.g. quantity details of the shipment.

## Background

### ONE Record Standard

The implementation of shipment tracking as described in this good practice is based entirely on the [ONE Record standard](https://github.com/IATA-Cargo/ONE-Record).

This good practice incorporates data classes of the [ONE Record cargo ontology v3.0.0](https://onerecord.iata.org/ns/cargo)
and the [ONE Record core code lists ontology v0.0.3](https://onerecord.iata.org/ns/coreCodeLists).

Furthermore, it utilises the [ONE Record API specificaiton v2.0.0](https://iata-cargo.github.io/ONE-Record/).

### Related Good Practices

The [ShipmentTracking](https://github.com/digital-cargo/good-practice-shipment-tracking) use case is closely related to the [shipment record](https://github.com/digital-cargo/good-practice-shipment-record) use case which is also based on the ONE Record standard. However, the ShipmentTracking use case is limited to the exchange of shipment status information with other parties, while the ShipmentRecord use case concerns the exchange of all shipment-related data with other parties, comparabel to today's (M)AWB and HAWB data. 
Thus, some of the data classes and data properties are used in both use cases.

### Piece-centricity and physics-orientation

Today in air cargo, tracking information is typically provided at the shipment level, but the ONE Record data model follows the principle of piece-centricity as a core design principle.
Another design principle of ONE Record is its aim to reflect the actual physical world, its objects and activities. 
For example, in ONE Record, it is not a legal object or a paper document such as the Air Waybill (AWB) that marks the progress of a shipment and reaches a milestone. 
Instead, it is the actual [Piece](https://onerecord.iata.org/ns/cargo#Piece), the wrapping [Shipment](https://onerecord.iata.org/ns/cargo#Shipment), or a [TransportMovement](https://onerecord.iata.org/ns/cargo#TransportMovement) activity that reaches a milestone in the journey. 
For example, when every piece in a shipment has been loaded and the aircraft departs, we consider the entire shipment as having departed.

### skeletonIndicator

The [skeletonIndicator](https://onerecord.iata.org/ns/cargo#skeletonIndicator) is a specific marker or flag used within data objects in the ONE Record standard. 
The [skeletonIndicator](https://onerecord.iata.org/ns/cargo#skeletonIndicator) signifies that the data object and its properties act as placeholders and do not represent granular, individual data
points. Instead, they offer a high-level or "skeletal" representation of the data, primarily for modeling piece-level data.

It enables piece-level modeling of shipment data in a not fully piece-level environment that is in transition, 
but provides the basis for future developments. 
This can be useful (1) when piece-level granularity is not required, (2) when non-integrable data sets are involved, 
(3) or when piece-level processing is not yet feasible in physical handling operations.

### Shipment Milestones

Baseline for this use case is sharing the same tracking information shared today by freight status update (FSU) messages in Cargo-IMP/Cargo-XML-based messaging. 
Additional information that goes beyond these standards can be exchanged ( for example, the FIW/FOW milestones, which cannot be exchanged in the current cIMP standard). 

The following table lists typical shipment milestones that SHOULD be supported in ShipmentTracking implementations. 
Depending on the type of LogisticsEvents, different types of LogisticsOjects are linked to them for ShipmentTracking. 

| Milestone, description                                                   | Linked ONE Record objects            | ONE Record LogisticsEvent example                             |
| ------------------------------------------------------------------------ | ------------------------------------ | ------------------------------------------------------------- |
| ARR: arrived on a specific flight                                        | Shipment / Piece / TransportMovement |                                                               |
| AWD: arrival documents have been delivered to the consignee or his agent | Shipment / Piece                     |                                                               |
| AWR: documents arrived on a given flight at destination airport          | Shipment / Piece / TransportMovement |                                                               |
| BKD: booked on a specific flight                                         | Shipment                             | [logistics-event-BKD.json](./assets/logistics-event-BKD.json) |
| CCD: Consignment cleared by customs                                      | Shipment / Piece                     |                                                               |
| CRC: reported to customs                                                 | Shipment / Piece                     |                                                               |
| DDL: delivered to consignee door                                         | Shipment / Piece                     |                                                               |
| DEP: departed on a specific flight                                       | Shipment / Piece / TransportMovement |                                                               |
| DIS-DFLD: Definitely loaded                                              | Shipment / Piece                     |                                                               |
| DIS-FDAV: Found mail                                                     | Shipment / Piece                     |                                                               |
| DIS-FDAW: Found air waybill                                              | Shipment / Piece                     |                                                               |
| DIS-FDCA: Found cargo                                                    | Shipment / Piece                     |                                                               |
| DIS-FDMB: Found mailbag                                                  | Shipment / Piece                     |                                                               |
| DIS-MSAV: Missing mail                                                   | Shipment / Piece                     |                                                               |
| DIS-MSAW: Missing mailbag                                                | Shipment / Piece                     |                                                               |
| DIS-MSCA: Missing cargo                                                  | Shipment / Piece                     |                                                               |
| DIS-OFLD: Off loaded                                                     | Shipment / Piece / TransportMovement |                                                               |
| DIS-OVCD: Over carried                                                   | Shipment / Piece / TransportMovement |                                                               |
| DIS-SSPD: Short shipped                                                  | Shipment / Piece                     |                                                               |
| DLV: Consignment delivered to the consignee or agent                     | Shipment / Piece                     |                                                               |
| DOC: Documents received by Handling Party                                | Shipment / Piece                     |                                                               |
| FIW: Freight Into Warehouse Control                                      | Shipment / Piece                     |                                                               |
| FOH: Freight on Hand                                                     | Shipment / Piece                     | [logistics-event-FOH.json](./assets/logistics-event-FOH.json) |
| FOW: Freight Out of Warehouse Control                                    | Shipment / Piece                     |                                                               |
| MAN: manifested on a specific flight                                     | Shipment / Piece / TransportMovement |                                                               |
| NFD: arrived at destination and the consignee or agent has been informed | Shipment / Piece                     |                                                               |
| PRE: prepared for loading on a specific flight                           | Shipment / Piece / TransportMovement |                                                               |
| RCF: received from a given flight                                        | Shipment / Piece / TransportMovement |                                                               |
| RCS: received from shipper or agent                                      | Shipment / Piece                     |                                                               |
| RCT: received shipments transferred from other carriers                  | Shipment / Piece                     |                                                               |
| TFD: Consignment transferred to another airline                          | Shipment / Piece                     |                                                               |
| TGC: transferred to Customs/Government control                           | Shipment / Piece                     |                                                               |
| TRM: Consignment to be transferred to another airline                    | Shipment / Piece                     |                                                               |

_(sorted alphabetically)_

## Data Provisioning

### Data Model

**Class Diagam**

This good practice incorporates data classes of the [ONE Record cargo ontology](https://onerecord.iata.org/ns/cargo) 
and the [ONE Record core code lists ontology](https://onerecord.iata.org/ns/coreCodeLists).
For clarity, class inheritance and unused data properties are excluded, and only required properties and relationships are visualized in the following.

The following class diagram shows the LogisticsObject data classes used and their relationships to the LogisticsEvent data class in the context of ShipmentTracking.

```mermaid
    classDiagram

    direction LR   
    class CodeListElement {
        + code: xsd:string
        + codeDescription: xsd:string
        + codeListReference: xsd:string
    }
    class Booking~LogisticsService~ {
        + bookingRequest: BookingRequest
        + issuedForWaybill: Waybill
        + skeletonIndicator: xsd:boolean [0..1]
    }
    Booking "1" --> "1" Waybill: issuedForWaybill
    Booking "1" --> "1" BookingRequest: bookingRequest

    class BookingRequest~LogisticsObject~ {
        + booking: Booking
        + forBookingOption: BookingOption
        + skeletonIndicator: xsd:boolean [0..1]
    }
    BookingRequest "1" --> "1" Booking: booking
    BookingRequest "1" --> "1" BookingOption: forBookingOption
    
    class BookingOption~LogisticsObject~ {
        + bookingTimes: BookingTimes
        + carrierProduct: CarrierProduct
        + forBookingRequest: BookingRequest
        + skeletonIndicator: xsd:boolean [0..1]
    }

    BookingOption "1" --> "1" BookingRequest: forBookingRequest
    BookingOption "1" --> "0..1" CarrierProduct: carrierProduct
    BookingOption "1" --> "0..1" BookingTimes: bookingTimes
  
    class CarrierProduct {
        + productCode: CodeListElement
        + productDescription: xsd:string
    }
    CarrierProduct "1" --> "1" CodeListElement: productCode

    class BookingTimes {
        + latestAcceptanceTime: xsd:dateTime
        + timeOfAvailability: xsd:dateTime
    }    
    
    class Waybill {           
      + arrivalLocation: Location
      + departureLocation: Location     
      + referredBookingOption: Booking
      + shipment: Shipment
      + waybillNumber: xsd:string
      + waybillPrefix: xsd:string
      + waybillType: WaybillType
    }
    Waybill "1" --> "1" Location: arrivalLocation     
    Waybill "1" --> "1" Location: departureLocation     
    Waybill "1" --> "1" Booking: referredBookingOption
    Waybill "1" --> "1" WaybillType: waybillType          
    Waybill "1" --> "1" Shipment: shipment                

    class Shipment {        
      + events: LogisticsEvent [0..*]
      + pieces: Piece [1..*]
      + totalGrossWeight: Value      
      + waybill: Waybill
    }  
    Shipment "1" --> "1..*" Piece : pieces
    Shipment "1" --> "1" Waybill: waybill
    
    class Piece {
      + events: LogisticsEvent [0..*]
      + grossWeight: Value      
      + involvedInActions: LogisticsAction [0..*]
      + ofShipment: Shipment
      + skeletonIndicator: xsd:boolean [0..1]
    }    
    Piece "1" --> "1" Value: grossWeight    
    Piece "1" --> "*" Loading: involvedInActions
    Piece "1" --> "1" Shipment: ofShipment
    

    class Organization {
      + name: xsd:string      
    }

    class Loading~LogisticsAction~ {      
      + loadedPieces: Piece [1..*]
      + servedActivity: LogisticsActivity
    }
    Loading "1" --> "1..*" Piece: loadedPieces     
    Loading "1" --> "1" TransportMovement: servedActivity     

    class TransportMovement~LogisticsActivity~ {
      + actions: Loading [1..*]
      + arrivalLocation: Location
      + departureLocation: Location
      + events: LogisticsEvent [0..*]
      + modeCode: ModeCode
      + modeQualifier: ModeQualifier
      + transportIdentifier: xsd:string            
    }
    TransportMovement "1" --> "1..*" Loading : actions     
    TransportMovement "1" --> "1" Location : arrivalLocation     
    TransportMovement "1" --> "1" Location : departureLocation     
    TransportMovement "1" --> "1" ModeCode : modeCode          
    TransportMovement "1" --> "1" ModeQualifier : modeQualifier          

    class Value {
      numericalValue: xsd:double
      unit: MeasurementUnitCode
    }
    Value "1" --> "1" MeasurementUnitCode : unit          

    class Location {
      + locationCode: CodeListElement
      + locationType: xsd:string            
    }
    Location "1" --> "1" CodeListElement: locationCode

    class WaybillType {
        <<Enumeration>>
        DIRECT
        MASTER
        HOUSE
    }

    class ModeQualifier {
        <<Enumeration>>
        MAIN_CARRIAGE
        ON_CARRIAGE
        PRE_CARRIAGE
    }
      class ModeCode {
        <<Enumeration>>
        AIR_TRANSPORT
        ROAD_TRANSPORT
        TRANSPORT_MODE_NOT_APPLICABLE
        TRANSPORT_MODE_NOT_SPECIFIED
        ...
    }
    class MeasurementUnitCode {
        <<Enumeration>>
        MeasurementUnitCode_KGM
        ...
    }

    class LogisticsEvent {
    }

    Shipment "1" --> "0..*" LogisticsEvent: events
    Piece "1" --> "0..*" LogisticsEvent: events
    TransportMovement "1" --> "0..*" LogisticsEvent: events
    LogisticsEvent "1" --> "1" Organization: recordedBy
```

The following class diagram visualizes the LogisticsEvent data class:

```mermaid
    classDiagram

    direction LR   
    
    class Location {
    
    }

    class LogisticsObject {
        + events: LogisticsEvent [0..*]
    }
    LogisticsObject "1" --> "0..*" LogisticsEvent: events
    
    class LogisticsEvent {
      + creationDate: xsd:dateTime
      + eventCode: CodeListElement
      + eventDate: xsd:dateTime
      + eventFor: LogisticsObject
      + eventTimeType: EventTimeType
      + partialEventIndicator: xsd:boolean [0..1]
      + recordedAtLocation: Location
      + recordedBy: Organization      
    }

    LogisticsEvent "1" --> "1" StatusCode: eventCode
    LogisticsEvent "1" --> "1" LogisticsObject : eventFor          
    LogisticsEvent "1" --> "1" EventTimeType : eventTimeType          
    LogisticsEvent "1" --> "1" Location : recordedAtLocation          
    LogisticsEvent "1" --> "1" Organization : recordedBy          

    class Organization {
      + name: xsd:string      
    }

    class EventTimeType {
        <<Enumeration>>
        ACTUAL
        PLANNED
    }
      
      class StatusCode {
        <<Enumeration>>
        StatusCode_ARR
        StatusCode_BKD
        StatusCode_DEP
        StatusCode_DIS_DFLD
        StatusCode_DIS_FDAV
        StatusCode_DIS_FDAW
        StatusCode_DIS_FDCA
        StatusCode_DIS_FDMB
        StatusCode_DIS_MSAV
        StatusCode_DIS_MSAW
        StatusCode_DIS_MSCA
        StatusCode_DIS_MSMB
        StatusCode_DIS_OFLD
        StatusCode_DIS_OVCD
        StatusCode_DIS_SSPD
        StatusCode_DLV
        StatusCode_FIW
        StatusCode_FOH
        StatusCode_FOW
        StatusCode_MAN
        StatusCode_NFD
        StatusCode_PRE
        StatusCode_RCF
        StatusCode_RCS
        ...        
    }
```

### Data Mapping

This section covers modelling and usage of classes and data elements from ONE Record data model, in particular the modelling of shipment status (also referred to as milestone or event) information 
on shipment-level and on piece-level, and considering specific scenarios, such as split shipments - shipments handled or moved in different parts - and transit shipments.

Other than ONE Record, the data structure supported and used by a Transport Management System (TMS) and other applications 
involved in ShipmentTracking related data exchange might not (yet) support the piece centric concept. Moreover, 
there is usually no dedicated distinction between physical, contractual and other categories the data is related to. 

This also applies to traditional messaging standards such as Cargo-IMP, Cargo-XML, internal web services, etc.
Especially during the transition period it might therefore be required to convert data between 1R standard and other 
data formats and data structures.

For this purpose, the target data format and guidelines for mapping between the different data formats must be jointly defined and applied.

Apart from the need of defining one dimensional mapping rules between the concerned data elements, general directions.
of how to organize transferring data between those different structures must be defined.

### Implementation Guidelines

This section outlines mandatory and best practice guidelines for the ShipmentTracking use case in accordance with the ONE Record standard. 
For every data class and property, compliance requires adherence to certain guidelines marked as MUST, while it is RECOMMENDED to follow others for best practices. 
Additionally, to facilitate comprehension, practical data examples are included to demonstrate the implementation of these guidelines.

**Location**

- A Location data object is a special LogisticsObject because it has a long lifespan and is linked comparatively often. Therefore, a location object SHOULD only be created once and then only referenced.
- It is possible that the same or a similar location is referenced by different organizations with different @id, e.g. because they are hosted on different servers. For example, a TransportMovement (on the ONE Record server of a carrier) refers to an FRA location, while a waybill (on the ONE Record server of a forwarder) also refers to an FRA location. In this case, both locations can have different @id. However, it is RECOMMENDED to refer to the same location (represented by the same @id) wherever possible.
- If only one data holder shares the data (variant 1 and variant 2), the @id of the Location object is the same.
- Since a Location object is typically stable yet frequently referenced master data, it is RECOMMENDED to choose an easily recognizable `@id`.
For instance, use `https://1r.example.com/logistics-objects/FRA` to represent Frankfurt Airport.
- For ShipmentTracking, besides the `@id` only the [locationCode](https://onerecord.iata.org/ns/cargo#locationCode) property MUST be set.
- Location data objects can be created ad-hoc during the data provisioning of shipment tracking data, e.g. when a new Location is referenced in a TransportMovement or Waybill object, or they can be created in advance, e.g. once during the initial setup of the ONE Record server and afterwards when a new location is added to the logistics network.

```json
{
    "@context": {
        "@vocab": "https://onerecord.iata.org/ns/cargo#"
    },
    "@type": "Location",
    "@id": "https://1r.example.com/logistics-objects/FRA",
    "locationCode": {
        "@type": "CodeListElement",
        "code": "FRA",
        "codeListName": "IATA airport codes"
    }
}
```
([location-FRA.json](./assets/location-FRA.json))

**Waybill**

- It is RECOMMENDED to use a defined schema for generation of @id of Waybill, e.g. using UUID v5 method in combination with a constant namespace UUID for all logistics objects of type Waybill and the waybill number as name.
See [UUID Version-5 Generator](https://www.uuidtools.com/v5), e.g. uuid5(namespace=6d5e79fa-3c9e-4e44-b4f0-b44cc5920f01, name=020-12345675-1) = 0615e450-ad51-552b-b512-45ae433ba3dd

```json
{
    "@context": {
        "@vocab": "https://onerecord.iata.org/ns/cargo#"
    },
    "@id": "https://1r.example.com/logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c",
    "@type": "Waybill",
    "arrivalLocation": {
        "@id": "https://1r.example.com/logistics-objects/JFK"
    },
    "departureLocation": {
        "@id": "https://1r.example.com/logistics-objects/FRA"
    },
    "shipment": {
        "@id": "https://1r.example.com/logistics-objects/8a76ed85-959e-45d5-8c42-5fd39c08efb1"
    },
    "waybillNumber": "12345675",
    "waybillPrefix": "020",
    "waybillType": {
        "@id": "https://onerecord.iata.org/ns/cargo#MASTER"
    }
}
```
([waybill.json](./assets/waybill.json))

**Shipment**

```json
{
    "@context": {
        "@vocab": "https://onerecord.iata.org/ns/cargo#"
    },
    "@id": "https://1r.example.com/logistics-objects/8a76ed85-959e-45d5-8c42-5fd39c08efb1",
    "@type": "Shipment",
    "pieces": [
        {
            "@id": "https://1r.example.com/logistics-objects/21ed25ef-4ef9-45ac-9088-b003d32ded95"
        }
    ],
    "totalGrossWeight": {
        "@type": "Value",
        "value": {
            "@type": "http://www.w3.org/2001/XMLSchema#double",
            "@value": "100"
        },
        "unit": {
            "@id": "https://onerecord.iata.org/ns/coreCodeLists#MeasurementUnitCode_KGM"
        }
    },
    "waybill": {
        "@id": "https://1r.example.com/logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c"
    }
}
```
([shipment.json](./assets/shipment.json))

**Piece**

```json
{
    "@context": {
        "@vocab": "https://onerecord.iata.org/ns/cargo#"
    },
    "@id": "https://1r.example.com/logistics-objects/21ed25ef-4ef9-45ac-9088-b003d32ded95",
    "@type": "Piece",
    "ofShipment": {
        "@id": "https://1r.example.com/logistics-objects/8a76ed85-959e-45d5-8c42-5fd39c08efb1"
    },
    "skeletonIndicator": {
        "@type": "http://www.w3.org/2001/XMLSchema#boolean",
        "@value": "true"
    },
    "grossWeight": {
        "@type": "Value",
        "value": {
            "@type": "http://www.w3.org/2001/XMLSchema#double",
            "@value": "100"
        },
        "unit": {
            "@id": "https://onerecord.iata.org/ns/coreCodeLists#MeasurementUnitCode_KGM"
        }
    }
}
```
([piece.json](./assets/piece.json))

**Loading**

```json
{
    "@context": {
        "@vocab": "https://onerecord.iata.org/ns/cargo#"
    },
    "@id": "https://1r.example.com/logistics-objects/5a4ade17-fe91-4d0c-bb79-8685a99d5634",
    "@type": "Loading",
    "loadedPieces": [{
        "@id": "https://1r.example.com/logistics-objects/21ed25ef-4ef9-45ac-9088-b003d32ded95"
    }],
    "servedActivity": {
        "@id": "https://1r.example.com/logistics-objects/bfcae0d4-9a29-4e60-880d-213aac434776"
    }
}
```
([loading.json](./assets/loading.json))

- For ShipmentTracking, the Loading data object is required to establish a connection between Pieces and the TransportMovements

**TransportMovement**

- For ShipmentTracking, every TransportMovement MUST have a [transportIdentifier](https://onerecord.iata.org/ns/cargo#transportIdentifier) property with the following structure:
`{carrier code in capital letters as two 2-digit code}{flight number 3-digit to 5-digit}{optional suffix}/{departure date as DDMMMyyyy}` or as regular expression:
`([A-Z]{2}|[A-Z\d]{2})\d{3-5}[A-Z]?\/\d{2}[A-Z]{3}\d{4}`. Examples: LH100S/16OCT2023, S72510/02NOV2023
- [arrivalLocation](https://onerecord.iata.org/ns/cargo#arrivalLocation) property MUST be a link to a Location data object
- [departureLocation](https://onerecord.iata.org/ns/cargo#departureLocation) property MUST be a link to a Location data object

```json
{
    "@context": {
        "@vocab": "https://onerecord.iata.org/ns/cargo#"
    },
    "@id": "https://1r.example.com/logistics-objects/bfcae0d4-9a29-4e60-880d-213aac434776",
    "@type": "TransportMovement",
    "actions": {
        "@id": "https://1r.example.com/logistics-objects/5a4ade17-fe91-4d0c-bb79-8685a99d5634"
    },
    "arrivalLocation": {
        "@id": "https://1r.example.com/logistics-objects/JFK"
    },
    "departureLocation": {
        "@id": "https://1r.example.com/logistics-objects/FRA"
    },
    "transportIdentifier": "LH400/16OCT2023"
}
```
([transport-movement-LH400.json](./assets/transport-movement-LH400.json))

**LogisticsEvent**

- LogisticsEvents are created in the context of a LogisticsObject, which MUST be consider when generating its `@id` property. The `@id` of a LogisticsEvent MUST be generated by using the `@id` of the LogisticsObject and appending `/logistics-events/{logisticsEventId}`, for example, `https://1r.example.com/logistics-objects/8a76ed85-959e-45d5-8c42-5fd39c08efb1/logistics-events/23e4d5f6-959e-45d5-8c42-5fd39c08efb1`
- For ShipmentTracking, the [eventTimeType](https://onerecord.iata.org/ns/cargo#eventTimeType) property MUST be set to [ACTUAL](https://onerecord.iata.org/ns/cargo#ACTUAL) or [PLANNED](https://onerecord.iata.org/ns/cargo#PLANNED). However, an LogisticsEvent with eventCode BKD MUST be only of eventTimeType [ACTUAL](https://onerecord.iata.org/ns/cargo#ACTUAL).
- For the [eventCode](https://onerecord.iata.org/ns/cargo#eventCode) property, a NamedIndividual from the [ONE Record core code lists ontology](https://onerecord.iata.org/ns/coreCodeLists) MUST be used.
- The [partialEventIndicator](https://onerecord.iata.org/ns/cargo#partialEventIndicator) property MUST only be used when some - but not all - pieces of a shipment have reached the milestone. In this case, this property MUST be set to `true` to indicate a partially reached milestone.
- The [recordedAtLocation](https://onerecord.iata.org/ns/cargo#recordedAtLocation) property MUST be a link to a [Location](https://onerecord.iata.org/ns/cargo#Location) data object.

For shipment tracking, Status Event Code, Reason Code (for DIS) and Partial ID (shipment level only) in data element <eventCode>
- concepts to map planning and actual status information
  - info from BKD status / booking info to be used for planned milestones
  - info from other status codes to be used for actual milestones


The following shows an example for a completes departure (DEP) milestone, without the [partialEventIndicator](https://onerecord.iata.org/ns/cargo#partialEventIndicator) property:
```json
{
    "@context": {
        "@vocab": "https://onerecord.iata.org/ns/cargo#"
    },
    "@type": "LogisticsEvent",
    "@id": "https://1r.example.com/logistics-objects/8a76ed85-959e-45d5-8c42-5fd39c08efb1/logistics-events/23e4d5f6-959e-45d5-8c42-5fd39c08efb1",
    "eventTimeType": {
        "@id": "https://onerecord.iata.org/ns/cargo#ACTUAL"
    },
    "eventCode": {
        "@id": "https://onerecord.iata.org/ns/coreCodeLists#StatusCode_DEP"
    },    
    "eventDate": {
        "@type": "http://www.w3.org/2001/XMLSchema#dateTime",
        "@value": "2023-04-01T10:38:01.000Z"
    },    
    "recordedAtLocation": {        
        "@id": "https://1r.example.com/logistics-objects/FRA"        
    }
}
```
([logistics-event-DEP.json](./assets/logistics-event-DEP.json))

The following shows an example for a partial completed departure (DEP) milestone with the [partialEventIndicator](https://onerecord.iata.org/ns/cargo#partialEventIndicator) property set to `true`:

```json
{
    "@context": {
        "@vocab": "https://onerecord.iata.org/ns/cargo#"
    },
    "@type": "LogisticsEvent",
    "@id": "https://1r.example.com/logistics-objects/8a76ed85-959e-45d5-8c42-5fd39c08efb2/logistics-events/23e4d5f6-959e-45d5-8c42-5fd39c08efb2",
    "eventTimeType": {
        "@id": "https://onerecord.iata.org/ns/cargo#ACTUAL"
    },
    "eventCode": {
        "@id": "https://onerecord.iata.org/ns/coreCodeLists#StatusCode_DEP"
    },    
    "eventDate": {
        "@type": "http://www.w3.org/2001/XMLSchema#dateTime",
        "@value": "2023-11-02T10:38:01.000Z"
    },    
    "partialEventIndicator": {
        "@type": "http://www.w3.org/2001/XMLSchema#boolean",
        "@value": "true"
    },
    "recordedAtLocation": {        
        "@id": "https://1r.example.com/logistics-objects/FRA"        
    }
}
```
([logistics-event-DEP-partial.json](./assets/logistics-event-DEP-partial.json))

**Linking LogisticsObjects and LogisticsEvents**
A LogisticsEvent denotes a specific event within the shipment process. All shipments have a first departure event and a last arrive event. While it is theoretically possible within the ONE record model to attach LogisticsEvents to a LogisticsObject via the #events property, it is intended to query these events through a dedicated endpoint.

When all pieces of a shipment have departed, the shipment has departed.

### Examples

This section demonstrates the previously described [implementation guidelines](#implementation-guidelines) with examples.



**Refer to this legend to interpret the shapes used in examples:**
> - **Blue rectangle with solid blue line:** LogisticsObject (e.g. Shipment, Piece, TransportMovement)
> - **Blue rectangle with dashed yellow line:** LogisticsObject with [skeletonIndicator](https://onerecord.iata.org/ns/cargo#skeletonIndicator)=true
> - **Green diamond with solid green line:** LogisticsEvent without [partialEventIndicator](https://onerecord.iata.org/ns/cargo#partialEventIndicator)
> - **Yellow diamond with dashed yellow line:** LogisticsEvent with [partialEventIndicator](https://onerecord.iata.org/ns/cargo#partialEventIndicator)=true
> - **(Plan)** indicates that the LogisticsEvent is a planned milestone
> - **(Act)** indicates that the LogisticsEvent is an actual milestone
![Data Mapping Examples Legend](./assets/data-mapping-examples-legend.png)


#### Example 1a: Shipment with one piece (only planned milestones)

![Example 1a: Shipment with one piece (only planned milestones)](./assets/data-mapping-example-1a.png)

Example 1a shows a shipment with only one piece that has reached the milestone Booked (BKD). 
In addition, the milestones Freight on Hand (FOH) and Received from Shipper (RCS) are planned for the shipment and the piece.

#### Example 1b: Shipment with one piece (planned and actual milestones)

Example 1a is an extension of Example 1a and shows a situation where the milestones Freight on Hand (FOH) and Received from Shipper (RCS) are planned for the shipment and the piece were planned; and the shipment and its piece has reached the milestones: Booked (BKD), Freight on Hand (FOH), and Received from Shipper (RCS).

In legacy Cargo-IMP this status would be captured by the following FSU messages:

```
FSU/14 020-12345675FRAJFK/T1K100 BKD/16OCT1317/FRA/ACME
FSU/14 020-12345675FRAJFK/T1K100 FOH/16OCT1317/FRA/LCAG
FSU/14 020-12345675FRAJFK/T1K100 RCS/16OCT1317/FRA/LCAG
```

![Example 1b: Shipment once piece (planned and actual milestones)](./assets/data-mapping-example-1b.png)

#### Example 2a: Shipment with one piece and flight specific status

![Example 2a: Shipment with one piece with flight specific status](./assets/data-mapping-example-2a.png)

The shipment has reached five miletones: BKD, FOH, RCS, MAN, DEP
The piece has reached four milestones: FOH, RCS, MAN, DEP
Even if not necessary to linked logistics events to the TransportMovement object, it is helpful for data consumption to compare the eventCode and eventTime of the MAN or DEP LogisticsEvent.
The same MAN and DEP LogisticsEvents are created for the transport movement.

In legacy Cargo-IMP this status would be captured by the following FSU messages:
```
FSU/14 020-12345675FRAJFK/T1K100 BKD/16OCT1317/FRA/ACME
FSU/14 020-12345675FRAJFK/T1K100 FOH/16OCT1317/FRA/LCAG
FSU/14 020-12345675FRAJFK/T1K100 RCS/16OCT1317/FRA/LCAG
FSU/14 020-12345675FRAJFK/T1K100 MAN/LH400/16OCT/FRAJFK
FSU/14 020-12345675FRAJFK/T1K100 DEP/LH400/16OCT/FRAJFK/T1K100
```

#### Example 2b: Two shipments with one piece each (planned on same flight)

![Example 2b: Two shipments with one piece each and flight specific status](./assets/data-mapping-example-2b.png)

#### Example 2c: Rescheduled shipment with one piece

![Example 2c: Rescheduled shipment with one piece](./assets/data-mapping-example-2c.png)


#### Example 3: Shipment with two pieces and different status each

![Example 3: Shipment with two pieces and different status each](./assets/data-mapping-example-3.png)

This example demonstrates the use of the [partialEventIndicator](https://onerecord.iata.org/ns/cargo#partialEventIndicator).
A shipment gets a LogsticsEvent only if all pieces of a shipment have reached a milestone. 
If this is not the case, a LogsticsEvent with the `partialEventIndicator = true` is added to the shipment data object. 
Later, when all pieces of a shipment have reached a milestone, an additional LogisticsEvent without the [partialEventIndicator](https://onerecord.iata.org/ns/cargo#partialEventIndicator) property is added to the shipment data object.
Pieces will always have LogisticsEvent data objecs without the [partialEventIndicator](https://onerecord.iata.org/ns/cargo#partialEventIndicator).


#### Example 4: Split Shipment with two pieces

![Example 4: Split Shipment with two pieces](./assets/data-mapping-example-4.png)

The shipment is reached the MAN milestone.
Only one piece reached the DEP miletone. Therefore the shipment only has a DEP milestone with the `partialEventIndicator = true`
When the second piece reached the DEP milestone, the shipment will get a second DEP milestone with the `partialEventIndicator = true`, and a third DEP milestone without the `partialEventIndicator`


#### Example 5a: Transit Shipment with one piece, completely manifested

![Example 5a: Transit Shipment with one piece, completely manifested](./assets/data-mapping-example-5a.png)

This example demonstrates the benefits of having LogisticsEvents also added to the TransportMovements
by matching MAN#1 of Piece #1 with MAN#1 of TransportMovement.


#### Example 5b: Transit Shipment with one piece, not completely manifested

![Example 5b: Transit Shipment with one piece, not completely manifested](./assets/data-mapping-example-5b.png)


#### Example 6: Split Transit Shipment with two pieces

![Example 6: Split Transit Shipment with two pieces](./assets/data-mapping-example-6.png)

#### Example 7: Planned Shipment with one piece and LAT and TOA

## Data Exchange

The ShipmenTracking use case is an easy starting point for a ONE Record transition for data providers and consumers. 
As it doesn't directly include the conclusion of a contract and can usually be considered as 
"one way communication", not all technical ONE Record features must be used.

### Endpoints

The ONE Record API provides a set of endpoints to exchange data. 
However, not all endpoints are required for the ShipmentTracking use case, e.g. the endpoints for audit trail, and change requests. 

In the following table we describe the ONE Record API endpoints required for ShipmentTracking:

| Resource / Endpoint                                                            | HTTP Action | Description                                                           |
| ------------------------------------------------------------------------------ | ----------- | --------------------------------------------------------------------- |
| /logistics-objects/{{logisticsObjectId}}                                       | GET         | Get LogisticsObject details                                           |
| /logistics-objects/{{logisticsObjectId}}/logistics-events                      | GET         | Get all LogisticsEvents of a LogisticsObject                          |
| /logistics-objects/{{logisticsObjectId}}/logistics-events/{{logisticsEventId}} | GET         | Get LogisticsEvent details                                            |
| /subscriptions                                                                 | GET         | Provide subscription information to publisher                         |
| /subscriptions                                                                 | POST        | Request a subscription for a LogisticsObject                          |
| /access-delegation                                                             | POST        | Request access delegation for a LogisticsObject and/or LogisticsEvent |
| /action-requests/{{actionRequestId}}                                           | GET         | Check status of subscription or access delegation request             |
| /action-requests/{{actionRequestId}}                                           | DELETE      | Revoke a pending subscription or access delegation request            |
| /notifications                                                                 | POST        | Receive shipment tracking updates                                     |

### Security

ShipmentTracking considers various types of security requirements.

- **Public access:** Also known as `Open Tracking API`, this offers the easiest to implement but least secured access. 
The data provider makes the data available to the public without verifying the identity and permissions. (no authentication required, no authorization required)  
- **Authenticated access** (authentication required): This level requires data consumers to prove their identity before accessing the tracking information. 
This adds a layer of security by ensuring that only recognized clients can interact with the API.
- **Authorized access** (authentication and authorization required): This requires that the requestor MUST present a valid identity and it is checked who is trying to access the API 
and whether the person has sufficient authorization to perform the request.

The final decision on which security requirements are required for a specific use case is made by the data provider.

Considering the following scenarios when selecting the level of security:
- non sensitive vs. sensitive information
  sensitive data e.g.:
  - Tracking info for Valuable or Vulnerable shipments;
  - content of (M)AWB contractual data, i.e. beyond flight routing, quantity details and shipment status
- wer darf welche Daten sehen, etc. FWD, GHA, andere / Identifizierung


- Because of the specificaiton ofthe standard, every request to a logistics-object needs to be authenticated by definition.
- The authoriation and access limitation is up to the implementer.

As for every public facing web API, it is RECOMMENDED to follow security best practices, including authentication, authorization, data encryption, and others, to ensure safe and secure data exchange.

For security reasons, it is RECOMMENDED to restrict access to logistics objects and logistics events to keep track of data access and data consumers.

For this use case, the authorization approach is left over to the implementing party. 
As of the nature of the "open" tracking API, authentication might not be required at all.

- Open: 
- Authentication required
- Authorization (incl. authentication) required

Example JWT Token (encoded):

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJhYTk4N2RjZS02YjUxLTExZWUtYjk2Mi0wMjQyYWMxMjAwMDIiLCJleHAiOjE2OTczNzA5OTYsImlzcyI6Imh0dHBzOi8vYXV0aC5leGFtcGxlLmNvbS9vYXV0aDIvZGVmYXVsdC92MS90b2tlbi90b2tlbiIsInN1YiI6IjEyMzQ1Njc4OTAiLCJsb2dpc3RpY3NfYWdlbnRfdXJpIjoiaHR0cDovLzFyLmV4YW1wbGUuY29tL2xvZ2lzdGljcy1vYmplY3RzL29yZ2FuaXphdGlvbi0xIn0.B7tYWhuVwscgHkmNOOGueNQ7D3uM0QXy6Al6OTuKZq4
```

Example JWT token (decoded payload):
```json
{
  "aud": "aa987dce-6b51-11ee-b962-0242ac120002",
  "exp": 1697370996,
  "iss": "https://auth.example.com/oauth2/default/v1/token/token",
  "sub": "1234567890",
  "logistics_agent_uri": "http://1r.example.com/logistics-objects/organization-1"
}
```

The ONE Record API can use the claim `logistics_agent_uri` in the JWT token to identify the data consumer and determine access rights to the requested resource.



The solution should principally "open" to maximize the user's benefits and minimize hurdles of implementation. 
This means that a basic layer of information should be available for data consumers without authentication. 
Some stakeholders might still require technical features like API keys for technical management, 
hurdles should be kept as low as possible for the user.

Although the base layer is as open as possible, additional, more sensitive information can be made available over the same API endpoints after an authentication. 
Thus, this use case is a good starting point for entering the ONE Record digital eco system.



### Required API implementation

The publisher's ONE Record API MUST implement the 
- GET LogisticsObject endpoint
- Subscribe endpoint

The subscriber's MUST implement a `/notifications` endpoint to receive Notifications. 

## Required functions

The following technical features are required on the data provider side:

- Implemented basic requests: GET, POST
- Generating and managing links for linked data
- Support publish and subscribe


On the data consumer side, even less functions are required for pure data consumption from the open tracking API:

- Making basic GET request
- Retrieving data from linked data sources

single ONE Record server / multiple ONE Record clients

> For each use case what is required: server, client, endpoints

## Request Shipment Tracking data

> For the sake of better comprehensibility, in the following examples it is assumed tht all data objects are 
> provided by a single data owner and hosted on a single ONE Record server, e. g. 1r.example.com
> This is the case in variant 1 and variant 2 where only one data holder shares the data. (cf. [data holder variants](#variants))
> In a real world environment, data objects are distributed across multiple ONE Record servers. These can be, 
> for example, carriers, ground handling agent (GHA), and other parties that also provide milestones and status updates
> along the supply chain. While the base URL component of the URIs may change, the API interactions remain the same.
> In some cases, additional API calls will be required by the data consumer to follow the linked data principle.

### LogisticsObject URI

Every Logistic Object as defined the [data mapping](#data-mapping) MUST have a globally unique id.
This good practice follows the defined structure of logistics object URIs which can be found in the [ONE Record API specification](https://iata-cargo.github.io/ONE-Record/concepts/#logistics-object-uri).

#### Waybill Specific LogisticsObject URI

For tokenized URIs, it is assumed that there is always a first contact between data provider and data consumer in a ONE Record world. 
This results in a specific problem for the given use case. For an "open" API, a previous contact with a subscription cannot be assumed. 
For the "first contact", the data consumer requires the unique tokenized ID for a shipment to request the data from the data owner. 
However, at this point the tokenized URI is not yet known or communicated. The data provider on the other side can not provide the tokenized URI 
because the data consumer is unknown due to the assumption of an "open API".
 
To solve this problem, for this specific use case, the URI for the GET request should contain the AWB number as the logisticsObjectId for the request. 
The following section describes a reference implementation.

### Use Case: Request shipment status for given waybill identifier

> **Note:**
> 
> "JSON-LD uses the same array representation as JSON, the collection is unordered by default. While order is preserved in regular JSON arrays, it is not in regular JSON-LD arrays unless specifically defined."
> 
> Source: [JSON-LD 1.1 specification](https://www.w3.org/TR/json-ld11/#terms-imported-from-other-specifications)

### data holder depending on entry point

1) Entrypoint is Waybill data object for which the data consumer knowns the LogisticsObjectURI, e.g. AWB number, e.g. 020-12345676. 
Data consumer can follow property #shipment in Waybill to get Shipment data and - if available - LogisticsEvents for a Shipment. 

This is special use case for air freight shipment tracking and is meant to be easier the transition.
Existing shipment tracking solutions allow to query for known shipment ids, e.g. the Air Waybill number.
We encourage everyone to setup a HTTP redirection that accepts the following path structure
`{scheme}://{host}[:port]/[basePath]/logistics-objects/{logisticsObjectId}`
as described in the [ONE Record API specification](https://iata-cargo.github.io/ONE-Record/concepts/#logistics-object-uri).

However, with the deviation that the `logisticsObjectId` URI component should follow the following:
`awb-{AWB prefix}-{AWB number}`


| Component  | Explanation                                                | Example  | Example explanation          |
| ---------- | ---------------------------------------------------------- | -------- | ---------------------------- |
| AWB prefix | provides the AWB prefix as part of the uniqueID of the AWB | 020      | Example of LH Cargo's prefix |
| AWB number | provides the AWB number as part of the uniqueID of the AWB | 12345675 | random example               |

This allows to query a ONE Record API as follows:

Request:
```http
GET /logistics-objects/awb-020-12345675 HTTP/1.1
Host: 1r.example.com
Accept: application/ld+json
```

Response:

```http
HTTP/1.1 307 Temporary Redirect
Location: https://1r.example.com/logistics-object/1a8ded38-1804-467c-a369-81a411416b7c
```


The following response serves as an example.

It is a strongly simplified data set with a shipment with one piece only on one transport movement and two events:


The response contains the actual location of the requested object using the Location http header. 
The client then has retrieved the unique tokenized ID of the Waybill and can get them by requesting the actual URI.

Request:

```http
GET /logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c HTTP/1.1
Host: 1r.example.com
Accept: application/ld+json
```

Response:

```bash
HTTP/1.1 200 OK
Content-Type: application/ld+json
Location: https://1r.example.com/logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c
Type: https://onerecord.iata.org/ns/cargo#Waybill
Revision: 1
Latest-Revision: 1

{
   "@context": {
      "@vocab": "https://onerecord.iata.org/ns/cargo#"
   },
   "@id": "https://1r.example.com/logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c",
   "@type": "Waybill",
   "waybillType": "https://onerecord.iata.org/ns/cargo#MASTER",
   "waybillNumber": "12345675",
   "waybillPrefix": "020",
   "shipment": {
      "@type": "Shipment",
      "@id": "https://1r.example.com/logistics-objects/8a76ed85-959e-45d5-8c42-5fd39c08efb1"
   }
}
```
_(This is a linked data representation of the Waybill)_

A ONE Record client can also request an embedded version of the Waybill using the `embedded` query parameter, , e.g.

```http
GET logistics-objects/1a8ded38-1804-467c-a369-81a411416b7c?embedded=true HTTP/1.1
Host: 1r.example.com
Accept: application/ld+json
```

Two important remarks on this:

1. Only data hosted on the same ONE Record server can be embedded, as otherwise the ownership control of another party would be violated.
2. Even if data is embedded, a link for every logistics object must be created additionally, to enable essential ONE Record features like audit trail, pub/sub, access control, etc. for these objects.

### Receive Notifications after Subscribe

#### Scenario 1 - Receive Complete Shipment Status after Notification

Sequence diagram
Prerequisite: Subscribed on shipment by publisher
A -> Notificaions -> B
A <- Get LogisticsEvents <- B

By receiving the non-partial LogisticsEvent for a milestone, 
the data consumer knows that all pieces of that shipment have reached that milestone.

#### Scenario 2 - Partial Shipment Status after Notification

Sequence diagram
Prerequisite: Subscribed on shipment by publisher
A -> Notificaions -> B
A <- Get LogisticsObject <- B
A <- Get LogisticsObject Piece A <- B
A <- Get LogisticsEvents <- B

By receiving a partial LogisticsEvent for a milestone,
the data consumer knwos that not the complete shipment, i.e. all pieces of that shipment, have reached that milestone.
Thus, if the data consumer wants to know which piece has reached or not yet reached the milestone, additional 
requests have to be made.


### Shipment data

The data field *Waybill#shipment* contains a link to a shipment. 
A shipment in ONE Record is the totality of physical entities under one contract. 
The *Shipment#totalGrossWeight* is a typical data field belonging in this object. 

```
### Piece linked to the shipment

Here, the piece has volume, dimensions and special handling codes (GEN, SPX and EAP).


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
  
## Shipment Tracking Subscribe

ShipmentTracking information can be shared with partners in two ways, either by pro-actively subscribing a known business partner or by request of a partner or 3rd party.

### Subscribe to Shipment Tracking Updates

In addition to a known business partner other parties might be interested in the shipment status, e.g. broker, consignee, ground handling agent, etc. 
These parties can actively subscribe by sending a subscription request for a specific shipment to its data owner / publisher.
After successful approval by the data owner, the publisher's ONE Record server begins sharing information with these parties whenever the shipping status information is updated.

The general mechanism of Subscription is described in the [ONE Record API specification](https://iata-cargo.github.io/ONE-Record/subscriptions/#subscribe-to-logistics-objects).

## Get subscribed to Shipment Tracking Updates 

**Subscription initied by party providing data:** For traditional FSU messaging transfer usually the relevant business partner, i.e. forwarding agent who has issued the AWB, is notified by the carrier proactively. This party usually also performs the booking and provides shipment data beforehand. In this context e.g. a completed booking and space allocation trigger submitting FSU messages to the business partner. For this purpose the carrier might maintain the messaging address (e.g. PIMA or SITA Address) in the internal customer database or any equivalent system.
A similar process is supported by One Record, i.e. notification process to a known business partner can be triggered by a dedicated shipment status, e.g. Booking completed - BKD. When e.g. BookingData and/or ShipmentRecord related data is shared via 1R beforehand this might be used as trigger.
Instead of a messaging address the OneRecord Server URI associated with that business partner is used to pass information to the right party. Similar to traditional messaging the carrier might have to maintain a list of URIs and related business partners for this purpose. 


## Guidelines for implementation

### Error Handling and ChangeRequest process

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

If applicable, ONE Record ChangeRequest process has to be applied when updating logistics objects for a certain shipment.
Relevance of 1R ChangeRequest process depends on the applied data exchange scenario. Please refer to details above.
For any logistics object related updates where equivalent data elements exist in traditional messaging specifications 
it is recommended of using the appropriate MIP "Error" Code along with ChangeIndicator "C" as specified in IATA Message 
Improvement Programme as reference.

When data is updated, shared or requested to be shared errors might occur, e.g. requested shipment id is unknown, server 
not available, logistics object update restricted due to different owner, etc. In general, the standard processes as specified in IATA 1R documentation apply.
For any logistics object related errors where equivalent data elements exist in traditional messaging specifications it is recommended of using the appropriate 
MIP Error Code as specified in IATA Message Improvement Programme as reference.

The 1R ChangeRequest process and Error Handling procss is described in the [ONE REcord API specification](https://iata-cargo.github.io/ONE-Record/) 
chapters [Subscriptions](https://iata-cargo.github.io/ONE-Record/subscriptions/), [Notifications](https://iata-cargo.github.io/ONE-Record/notifications/), and [Action Requests](https://iata-cargo.github.io/ONE-Record/action-requests/).

 



# Migration from Legacy Data Exchange
As of now ShipmentTracking related data has been exchanged mostly via Cargo-IMP FSU and FSA messages. 
These messages provide status information for dedicated events of the airport to airport process on (M)AWB level. 
Same applied to equivalent Cargo-XML messages.
In contrast to that the 1R data model is based on piece level. Moreover, via 1R logistics event related information 
can be provided for any logistics objects available in the 1R data model. This involves differences to both, 
methodologies of data exchange and structure of data.
This has to be considered when migrating data exchange to 1R and/or transferring data between 1R and traditional 
data interchange methods.
The attached mapping instructions shall help to understand these differences, explain how to use the 1R data model 
to exchange ShipmentTracking related data, as well as provide guidelines of converting data from and to 1R. 

Compared to existing exchanges, e.g. via CargoIMP/CargoXML, some milestones in a ONE Record environment are assigned to other objects - partly more fine-grained. 

The table [ONE-Record-CargoIMP-Mapping.xslx](/assets/ONE-Record-CargoIMP-Mapping.xslx) illustrates the relationship of data classes and their data properties in ONE Record with the legacy standard Cargo-IMP and CargoXML through a mapping.




## Glossary
see [digita-cargo/glossary](https://github.com/digital-cargo/glossary)

## References

- ...
- ...
- ...
  
## Acknowledgements

The initial version of this document is the outcome of the 
"Joint ONE Record piloting and transition working group // technical part" at IATA. 
It was orchestrated by Arnaud Lambert of IATA as secretary and [Philipp Billion](https://github.com/DrPhilippBillion) of Lufthansa Cargo as chairman.

Special thanks to [Niclas Scheiber](https://github.com/NiclasScheiber), Frankfurt University of Applied Sciences for preparing version 3.0.0 of the 
ONE Record core ontology in coordination with the IATA ONE Record data model focus group.

## Community

### Contribute

See [CONTRIBUTING](CONTRIBUTING.md) for more details on how to contribute on this good practice.

### Issues
Issues related to this good practice are tracked on GitHub

- [View open issues](https://github.com/digital-cargo/good-practice-shipment-tracking/issues)
- [Create a new issue](https://github.com/digital-cargo/good-practice-shipment-tracking/issues/new)

### Maintainers

> Each good practice MUST have at least one maintainer who is responsible for ongoing development and quality assurance. 
> Every maintainer MUST have commit access to the good practice repository.

- [Daniel A. Döppner](https://github.com/ddoeppner), Lufthansa Cargo 
- [Ingo Zeschky](https://github.com/ChrisKranich), Lufthansa Cargo
- [Philipp Billion](https://github.com/DrPhilippBillion), Lufthansa Cargo

_(sorted alphabetically)_

### Contributors

> Every good practice is the result of the work of the community, and therefore the contribution of each individual should be recognized and appreciated. 
> Below is a list of all the people who have actively contributed to this good practice.

- Ajay Manoharan, Qatar Airways
- Arnaud Lambert, IATA
- Bilel Chakroun, Air France-KLM
- [Hendrik Gruber](https://github.com/HendrikLH), Lufthansa Industry Solutions
- Josh Priebe, Air Canada
- Keith Lam, GLS HKG 
- Mark Belliss, British Telecom 
- Martin Fowler, MDF Solutions
- [Martin Skopp](https://github.com/mskopp), Riege Software
- Mary Stradling, DHL
- Matthias Hurst, Colog AG
- Pramod Rao, Nexshore Technologies
- [Ying Lu](https://github.com/luyinglu), Lufthansa Industry Solutions

_(sorted alphabetically)_


