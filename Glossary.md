# Glossary
## Transport Management System (TMS)
## ONE Record Client
## ONE Record Server
An implementation of the ONE Record API specification. This is often used to refer to an instance of a ONE Record API.
## ONE Record API
Application Programming Interface to exchange data.
## OpenID Connection (OIDC)
## JSON Web Token (JWT) 
## CargoIMP (cIMP)

Cargo-IMP (Interchange Message Procedures) was introduced by IATA (International Air Transport Association) in the 1970s. 
It was designed as a set of standardized electronic communication protocols to facilitate the exchange of cargo-related information between airlines, 
freight forwarders, and other stakeholders in the air cargo industry. 
Over the years, it became the industry standard for electronic data interchange (EDI) in air cargo operations. 
However, with the evolution of technology and the need for more modern and flexible messaging formats, 
IATA has been transitioning towards [CargoXML](#cargoxml-cxml) as a replacement for Cargo-IMP.

## CargoXML (cXML)

CargoXML is a standard developed by IATA for the air cargo industry, designed to modernize and replace the older Cargo-IMP format. 
Being an XML-based format, CargoXML messages are structured with nested tags and attributes, making them more flexible and easier to integrate with modern IT systems.

## Freight Status Update (FSU)

The Freight Status Update (FSU) message is a standard electronic communication used to notify involved parties of the status or change in status of a shipment. 
This electronic data interchange (EDI) message is part of the International Air Transport Association's (IATA) Cargo-IMP (Interchange Message Procedures) standards, which facilitate the exchange of cargo-related information between airlines, freight forwarders, and other air cargo stakeholders.

The FSU message contains detailed information about the shipment's status, such as when the cargo is received, loaded on an aircraft, or delivered to the consignee. Various status codes can be used within the FSU message to convey specific milestones or events related to the shipment, allowing for more efficient tracking and management of air cargo.

## Ground Handling Agent (GHA)
## Freight Forwarder (FF)
## Logistics Object
## Logistics Event

### IATA ONE Record

A digital standard developed by the International Air Transport Association (IATA) aiming to provide a unified approach to data sharing and storage in the air cargo industry.

### Data Model

A structured representation of data objects and their relationships, facilitating effective data processing, storage, and retrieval.

### API (Application Programming Interface)

A set of protocols and tools that allows different software applications to communicate with each other, enabling integration and data exchange.

### Legacy Systems

Older or traditional IT systems, software, or technologies that a business continues to use, often because they fulfill a business need, even as newer systems or technologies become available.

### Skeleton indicator

Introduced with the ONE Record Data model version 3.0.0, the `skeletonIndicator` is a specific marker or flag used within data objects 
in the IATA ONE Record standard. 

The `skeletonIndicator` signifies that the data object's values act as placeholders and do not represent granular, individual data points.
Instead, they offer a high-level or "skeletal" representation of the data, primarily for modeling piece-level data.


# For guidelines how to write good practice
### Open Access

The ethos of this good practice is rooted in promoting widespread accessibility and industry-wide adoption. 
As such, it is freely accessible, and we strongly encourage its dissemination throughout the industry. 
Should you find value and apply it in your operations, we kindly ask to add an reference to this document, accompanied by a link to this Github repository. 
Such recognition not only promotes the sharing of expertise, but also transparency and scope in the industry.