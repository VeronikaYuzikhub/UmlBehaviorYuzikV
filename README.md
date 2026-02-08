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
```


```mermaid
  sequenceDiagram
    autonumber
    actor U as User
    participant S as System
    participant PG as PaymentGateway
    participant DB as Database

    U->>S: Request to Book Room
    S->>DB: Check room availability
    DB-->>S: Room is free
    S-->>U: Confirm availability

    Note over U, PG: Payment Process
    U->>S: Request payment for room
    S->>PG: Process payment
    PG-->>S: Payment success
    S-->>U: Confirm payment

    S->>DB: Update state to "Reserved"
    DB-->>S: Success
    S-->>U: Provide Booking Confirmation
```

```mermaid
  sequenceDiagram
    autonumber
    actor U as User
    participant S as System
    participant PG as PaymentGateway
    participant DB as Database
    actor A as Admin

    Note over U, DB: Stage_1: Reservation & Payment
    U->>S: Request to Book Room
    
    Note right of S: include: Check availability
    S->>DB: Check availability
    DB-->>S: Status free
    S-->>U: Confirm availability
    
    Note over U, PG: Payment Process
    U->>S: Request payment for room
    S->>PG: Process payment
    PG-->>S: Payment successful
    S-->>U: Confirm payment success
    
    S->>DB: Update state to "Reserved"
    DB-->>S: Success
    S-->>U: Provide Booking Confirmation

    Note over S, A: Stage_2: Admin Processing
    
    A->>S: View all bookings
    S-->>A: Show list all bookings & Confirm

    opt Annul Booking
        A->>S: Annul reservation
        S->>PG: Request payment refund
        PG-->>S: Payment refunded
        S->>DB: Revert state to "Cancelled", "Free"
        DB-->>S: Success
        S-->>U: Notify Booking Cancelled & Refunded
    end
