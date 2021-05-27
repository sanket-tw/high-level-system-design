https://www.educative.io/courses/grokking-the-object-oriented-design-interview/gxM3gRxmr8Z
1. multi level ->. this says when we give ticket to user, it should have level and spot as well
2. multiple entry and exit points -> so the payment process, ticket generation should sync up, so we do not allot a parking to both, and also clear parking spot evenly from all places. a common database would do. Since read and write would be 1:2 (read if spot occupied, when payment done, release spot). so we need strong consistency, hence relational database. Locking while changing.
    Might have strategy as to where to allot parking
3. Entry- generate ticket, reserve spot, Exit- payment processing, release spot; log at both sides to common logger, monitoring( later)
4. Payment can be captured at 2 places, assitant is one role
5. Payment by various means, cash, credit, means capture.
6. Payment also at port info, payment record as part of ticket, sync with Exit, 
7. max capacity warning
8. parking floor spot with mixed vehicles
9. electric car parking, extra charge if opt for charging
10. vehicle type as well
11. per-hour parking fee model

Actors/Role:
	Admin
	System
	Customer
	Assistant:  can take payment, be at info panel payment, 
	
	
	
	Actions | Admin does all 
	1. generate ticket  S,A
	2. pay ticket C
	3. process payment A,S
	4. calculate amount S
	5. reserve spot S
	6. release spot S
	7. accept payment S,A
	
	Customer 1. Accept ticket
					2. scan ticker
					3. pay ticket
					4. Credit Card/Cash payment

	Attendant: 1. manage customer account, view, update, sys login and cash payment accept
	
	System: assign parketing spot, release spot, show full message, show available parking spot
	
	Admin: add floor, add spot, add parking attendat, add/update rate, add entry exit : modify infratstructure
	
	---------------------
	
	Parking Lot : id - Name, and address (singleton for now, if single but can be entity as well)
	
	ParkingFloor: has multiple spots, charging point, info panel, entry exit
	
	Parking Spot: id:, type, charging point, isOccupied, 
	
	Account: Admin, assiant, basic details, user login details, define access
	
	Ticket: id, parking spot, vehicle details, timeOfIssue, timeOfLeave, rateCard, payment details 
	
	Vehicle: license plate, customer details, type of parking required, electric or not
	
	Entry/Exit: id, Entry: generateTicket , Exit: process payment
	
	Payment: mode of payment, interface to other payment method (could be a service in its own, but have its own DB)
	
	ParkingRate: configs, id, timeOfIssue, (validTill, YAGNI?)
	
	ParkingDisplayBoard: outer interface, id, location, status, message. isWorking, lastUpdated
	
	ParkingAttendantPortal: has attendant, info about attendant logins and logout, floor ids, scan ticket, process payment
	
	CustomerInfoPanel: ssame as above, pay, update ticket. details.
	
	ElectricPanel: pay and charge vehicle, property of a ParkingSpot
	
	
	
	
	