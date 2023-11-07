Changelog 2023-11-03

- Changed guidelines for transportMovementIdentifier to capture airlines with alphanumeric 2-digit code and to capture suffixes in flight number  
- Added PLANNED as evenTimeType for LogisticsEvents in LogisticsEvents example and in data mapping
- Added restriction that BKD LogisticsEvents can only be actual.
- Renamed API design section to "data exchange"
- Added overview of ONE Record API endpoints
- Renamed FSU milestones to general shipment milestones
- Consolidated tables of milestones and sorted milestones aphabetically
- Added an implementation guideline that locations should be created once and linked afterward.-  
- Added note that LogisticsEvents due to JSON-LD Specification would not be ordered 
- Multiple Events of same type and eventCode; sort by creationDate descending and take first
- Renamed section "Migration from/to CargoXML" to "Migration from Legacy Data Exchange"
- Added reference to CargoXML-ONE Record mapping excel document
- Added legend for data mapping examples, changed the shapes and its colors (to address color-blindness) in the examples and adopted the legend accordingly
- Moved the legend to the top of the examples
- Added example for a replanned shipment
- Added example for actual and planned milestones
- added example for two shipments on same TransportMovement


- Data Mapping Fehlende Datenklasseen ergänzen
- /logistics-events eigenheit erklären bzw. #events in LogiticsObject
- TransportMovement wird einmal angelegt.

- However, in the ONE Record API 2.0.0, order is considered for pagination (e.g. using `limit` and `offset`)

- id generation of master data

- add example for a replan



- add guideline that same LogisticsEvents, i.e. same eventCode and eventTimeType, and location, should be ordered by creationDate descending and the first one should be taken; due to the immutabilty of LogisticsEvents, i.e. cannot be changed or deleted after creation,  in the ONE Record API, example, replanned Shipments, with two FOH events, take the last one by creationDate descending
- add security guideline that LogisticsEvents should be secured by the data holder of the linked LogisticsObject. This means that only the data holder and the data creator of the LogisticsEvent should be able to read the LogisticsEvent

- add discussion section
- known tradeoffs: duplicate data due to implicity, for example, a linked Loading LogisticsObject between a Piece and TransportMovement has the same information as the planned LogisticsEvents PRE and MAN


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
    },

    # Alternative 1
    "locationType": {
        "@type": "CodeListElement",
        "code": "AIRPORT",
        "codeListName": "Location type codes"
    }

    # Alternative 2
    "locationType": "AIRPORT",
    "locationType": "Airport";
    "locationType": "A",

    # Alternative 3
    "locationType": {
        "@id": "https://onerecord.iata.org/ns/coreCodeList#LocationType_AIRPORT"
    }

    # Alternative 4: something else?!
    
}
```
