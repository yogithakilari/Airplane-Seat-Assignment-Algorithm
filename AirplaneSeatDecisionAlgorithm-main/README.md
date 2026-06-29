# Airplane Seat Assignment Algorithm

This project is a Java-based airplane seat assignment algorithm.

The goal is to assign seats to airplane passengers using seat availability, passenger preferences, booking groups, passenger priority, paid seat selections, and airline safety rules.

## Problem Statement

Airlines need to assign seats in a way that is safe, fair, and practical.

The system should not simply pick the first available seat. It should consider:

- Seat availability
- Seat status
- Passenger age category
- Ticket class
- Seat preferences
- Paid seat selections
- Booking groups
- Loyalty priority
- Check-in priority
- Safety rules
- Group seating

## Approach Used

This project uses a constraint-based scoring approach.

First, the algorithm checks hard rules that cannot be broken.  
Then, it scores valid seat options and chooses the best allocation based on passenger preferences, group seating, paid seat selection, and priority.

## Flow Chart of the Algorithm

                                  ┌───────────────┐
                                  │     START     │
                                  └───────┬───────┘
                                          │
                                          ▼
                     ┌──────────────────────────────────┐
                     │ Read Input                       │
                     │ - Seat Layout                    │
                     │ - Number of Passengers           │
                     └───────────────┬──────────────────┘
                                     │
                                     ▼
                   ┌────────────────────────────────────┐
                   │ Create Airplane Seating Matrix     │
                   └───────────────┬────────────────────┘
                                   │
                                   ▼
              ┌─────────────────────────────────────────────┐
              │ Identify Seat Types                         │
              │ • Aisle Seats                               │
              │ • Window Seats                              │
              │ • Center Seats                              │
              └───────────────┬─────────────────────────────┘
                              │
                              ▼
               ┌────────────────────────────────────┐
               │ Allocate Aisle Seats First         │
               └───────────────┬────────────────────┘
                               │
                               ▼
                  ┌─────────────────────────┐
                  │ Aisle Seats Remaining ? │
                  └───────┬─────────┬───────┘
                          │Yes      │No
                          ▼         ▼
                ┌──────────────┐   ┌──────────────────────────┐
                │ Assign Seat  │   │ Allocate Window Seats    │
                └──────┬───────┘   └─────────────┬────────────┘
                       │                         │
                       └─────────────┐           ▼
                                     │   ┌──────────────────────────┐
                                     │   │ Window Seats Remaining ? │
                                     │   └──────┬──────────┬────────┘
                                     │          │Yes       │No
                                     │          ▼          ▼
                                     │   ┌─────────────┐  ┌──────────────────────┐
                                     │   │ Assign Seat │  │ Allocate Center Seats│
                                     │   └─────┬───────┘  └──────────┬───────────┘
                                     │         │                     │
                                     └─────────┘                     ▼
                                              ┌────────────────────────────┐
                                              │ Center Seats Remaining ?   │
                                              └──────┬───────────┬─────────┘
                                                     │Yes        │No
                                                     ▼           ▼
                                             ┌─────────────┐  ┌─────────────────────┐
                                             │ Assign Seat │  │ Display Final Seat  │
                                             └─────┬───────┘  │ Arrangement         │
                                                   │          └──────────┬──────────┘
                                                   └─────────────────────┘
                                                                  │
                                                                  ▼
                                                         ┌────────────────┐
                                                         │      END       │
                                                         └────────────────┘

## Hard Rules

The algorithm checks:

- Seat must be available
- Seat must not be occupied, blocked, or locked
- Paid-only seats can only be assigned to passengers who paid for that specific seat
- A child or infant cannot sit in an exit row
- An infant must sit only in an infant-allowed seat
- Passenger ticket class must match the seat class
- One seat cannot be assigned to multiple passengers

## Scoring Rules

The algorithm gives scores for better seat decisions:

- Correct cabin class: +500
- Same-row group seating: +400
- Adjacent-row group seating: +250
- Preference matched: +200
- Paid seat selection honored: +300
- Loyalty priority: higher loyalty gets higher score
- Check-in order: earlier check-in gets higher score
- Group split: negative score


## Project Structure

```text
AirplaneSeatAssignment/
|
|-- Seat.java
|-- Passenger.java
|-- Assignment.java
|-- AirplaneSeatAssignment.java
|-- .gitignore
|-- README.md
```

## How To Run

Compile the Java files:

```bash
javac *.java
```

Run the program:

```bash
java AirplaneSeatAssignment
```

## Sample Output

```text
Final Seat Assignments:
-----------------------
P1 -> 10A | paid seat selection honored, window preference matched, loyalty priority considered, check-in order considered.
P2 -> 10C | aisle preference matched, loyalty priority considered, check-in order considered.
P3 -> 10B | check-in order considered.
P4 -> 12A | extra-legroom preference matched, loyalty priority considered, check-in order considered.
```

## Classes Used

### Seat

Stores seat details such as:

- Seat number
- Row
- Seat type
- Seat class
- Availability
- Exit row status
- Infant allowed status
- Seat status
- Paid-only status
- Front-row flag
- Quiet-zone flag
- Extra-legroom flag

### Passenger

Stores passenger details such as:

- Passenger ID
- Age category
- Ticket class
- Seat preference
- Group ID
- Paid seat selection
- Loyalty priority
- Check-in order

### Assignment

Stores the final result:

- Passenger
- Seat
- Reason for assignment

### AirplaneSeatAssignment

Contains the main algorithm logic.

Main responsibilities:

- Validate hard rules
- Group passengers
- Sort passengers by priority
- Score valid seats
- Try same-row group seating
- Try adjacent-row fallback
- Try same-section fallback
- Apply fair tie-breaking
- Return final assignments with explanation

## Future Scope

The following production-level features can be added in future:

- Upgrade or manual authorization rules
- Wheelchair support rules
- Real-time seat locks with expiry
- Concurrent booking/check-in safety
- Aircraft swap or layout change handling
- Manual airline staff override
- Refund handling if paid seat selection cannot be honored
- Reading input from files, database, or API
- Unit tests

## Summary

This project starts with a simple working solution and improves step by step.

The final implementation is rule-based, preference-aware, priority-aware, group-friendly, and explainable.

It does not behave like a simple first-available-seat picker. Instead, it applies hard constraints first and then uses scoring to choose the best valid seat allocation.
