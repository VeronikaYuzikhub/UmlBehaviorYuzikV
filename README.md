# UmlBehaviorYuzikV
```mermaid
  graph LR
    User((User))
    Admin((Admin))

    subgraph "Hotel Booking System"
        UC1(View Available Rooms)
        UC2(Book Room)
        UC3(Check Availability)
        UC4(Payment)
        UC5(Cancel Reservation)
        
        UC6(Process Booking)
        UC7(Confirm Booking)
        UC8(Annul Booking)
        
        UC9(Manage Availability)
        UC10(Set 'Under Repair' Status)
        UC11(General Status Update)
    end

    %% User Connections
    User --> UC1
    User --> UC2
    User --> UC4
    User --> UC5

    %% Relations for User
    UC2 -.->|include| UC3
    UC2 -.->|extend| UC4

    %% Admin Connections
    Admin --> UC6
    Admin --> UC9
    Admin --> UC5

    %% Relations for Admin
    UC6 -.->|extend| UC7
    UC6 -.->|extend| UC8
    
    UC9 -.->|extend| UC10
    UC9 -.->|extend| UC11
