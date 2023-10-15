- add Section about Security
  - Because of the specificaiton ofthe standard, every request to a logistics-object needs to be authenticated by definition.
  - The authoriation and access limitation is up to the implementer.

It is RECOMMENDED to restrict access to logistics objects and logistics events to keep track of data access and data consumers.


# Piece-level topic in mapping

Integration with legacy systems
- Due to the heteregenous applcation landscape, specific translators for all standards can be given, 
therefore the practice gives only recommendations from legacy cargo messaging standards. 


# Scenario 1 - Receive Complete Shipment Status after Notification

Sequenzdiagram
Prerequisite: Subscribed on shipment by publisher
A -> Notificaions -> B
A <- Get LogisticsEvents <- B

By receiving the non-partial LogisticsEvent for a milestone, 
the data consumer knows that all pieces of that shipment have reached that milestone.

# Scenario 2 - Partial Shipment Status after Notification

Sequenzdiagram
Prerequisite: Subscribed on shipment by publisher
A -> Notificaions -> B
A <- Get LogisticsObject <- B
A <- Get LogisticsObject Piece A <- B
A <- Get LogisticsEvents <- B

By receiving a partial LogisticsEvent for a milestone,
the data consumer knwos that not the complete shipment, i.e. all pieces of that shipment, have reached that milestone.
Thus, if the data consumer wants to know which piece has reached or not yet reached the milestone, additional 
requests have to be made.


# Subscribe on LogisticsObjects to receive Shipment Tracking Notifications




