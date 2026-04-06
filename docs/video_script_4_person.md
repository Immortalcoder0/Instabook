# Bus Ticket Booking System - 4 Person Video Script

## Team Division

### Person 1 - Main Application + Database Setup
- `src/BusTicketApp.java`
- `src/util/DatabaseConnection.java`
- `src/config/db.properties`
- `sql/database_setup.sql`
- `sql/sample_data.sql`
- Overall project introduction and final execution flow

### Person 2 - DAO Layer
- `src/dao/RouteDAO.java`
- `src/dao/BusDAO.java`
- `src/dao/SeatDAO.java`
- `src/dao/BookingDAO.java`

### Person 3 - Model Layer
- `src/model/Route.java`
- `src/model/Bus.java`
- `src/model/Seat.java`
- `src/model/Booking.java`

### Person 4 - Service Layer
- `src/service/RouteService.java`
- `src/service/BusService.java`
- `src/service/SeatService.java`
- `src/service/BookingService.java`

## 10 Minute Video Flow

### Total Duration
- Person 1: 2 minutes 30 seconds
- Person 2: 2 minutes 15 seconds
- Person 3: 2 minutes
- Person 4: 3 minutes 15 seconds

## Video Script

### Person 1 Script - Introduction, Main App, Database Setup, Start of Execution

Good morning everyone. We are presenting our Java mini project, Bus Ticket Booking System. This is a console-based application developed using Java, JDBC, and MySQL. The main objective of this project is to allow users to view routes, search buses, check seat availability, book tickets, cancel bookings, and view booking details in a simple and user-friendly way.

I worked on the main application flow and the database setup. The main entry point of our project is `BusTicketApp.java`. This file acts like the controller of the whole system. It creates the scanner object, initializes all service classes, checks the database connection at startup, displays the main menu, and calls different methods based on the user’s choice.

In the `start()` method, we first display a welcome screen, then test whether the MySQL database is connected properly. If the connection fails, the system shows an error message and stops safely. If the connection is successful, the program enters the main menu loop. From there, the user can choose options like view all routes, search buses, view seat availability, book a ticket, cancel booking, search bookings, and exit.

For database setup, we used MySQL and created four main tables: `routes`, `buses`, `seats`, and `bookings`. These are defined in `sql/database_setup.sql`. We also created indexes for faster searching, a `booking_details` view for easy retrieval, one stored procedure to get available seats, and triggers to update seat status after booking and cancellation.

Then in `sql/sample_data.sql`, we inserted sample routes and buses, and generated seats automatically for each bus. So the system is ready with test data before execution.

Now I will also show how the project is executed. First, we configure the database inside `src/config/db.properties`. Then we compile and run the project using:

```cmd
javac -cp "lib/*" -d bin src/model/*.java src/util/*.java src/dao/*.java src/service/*.java src/*.java
java -cp "bin;lib/*" BusTicketApp
```

When we run the project, the application shows the welcome screen and the main menu. I will begin the demo by selecting option 1 to view all routes, so we can see the available source, destination, fare, and duration. After this, my teammate will explain how the DAO layer fetches this data from the database.

### Person 2 Script - DAO Layer

I worked on the DAO layer, which stands for Data Access Object layer. This layer is responsible for direct interaction with the MySQL database. In our project, we have four DAO classes: `RouteDAO`, `BusDAO`, `SeatDAO`, and `BookingDAO`.

The main purpose of this layer is to keep database queries separate from business logic. Because of this separation, our code is cleaner, easier to maintain, and follows good software design principles.

In `RouteDAO`, we implemented methods like `getAllRoutes()`, `getRouteById()`, and `searchRoutes()`. These methods execute SQL queries and convert database records into Java objects.

In `BusDAO`, we fetch buses based on route ID, get a bus by its ID, and also count how many seats are available on a particular date. This is important because seat availability depends not only on the bus, but also on the travel date.

In `SeatDAO`, we manage seat-related queries. It can fetch all seats, fetch available seats for a selected date, fetch only unbooked seats, and check if a seat is still available. One important method here is `isSeatAvailable(Connection conn, int seatId, Date travelDate)`, which uses `FOR UPDATE`. This helps lock the record during booking and prevents two users from booking the same seat at the same time.

In `BookingDAO`, we handle booking insert, cancellation, fetching booking by ID, viewing all confirmed bookings, and searching bookings using phone number. The `createBooking()` method inserts a new booking and returns the generated booking ID.

Now during the demo, when we select a route and search for buses, these DAO classes are the ones actually reading data from the `routes`, `buses`, `seats`, and `bookings` tables. So the DAO layer works as the bridge between Java code and MySQL database.

### Person 3 Script - Model Layer

I worked on the model layer. The model classes represent the main entities used in our system. In this project, we have four model classes: `Route`, `Bus`, `Seat`, and `Booking`.

The `Route` model stores route ID, source, destination, fare, and duration in minutes. It also has a helper method called `getFormattedDuration()` which converts total minutes into a readable format like 5 hours 30 minutes.

The `Bus` model stores information like bus ID, route ID, bus number, bus type, total seats, and departure time. It also includes extra display fields such as source, destination, and fare. Another useful method is `getBusTypeDisplay()`, which converts values like `NON_AC` into a user-friendly format like Non-AC.

The `Seat` model contains seat ID, bus ID, seat number, seat type, and booking status. It also has helper methods such as `getStatusDisplay()` and `getSeatTypeIcon()` for displaying seat information more clearly in the console interface.

The `Booking` model stores booking ID, seat ID, passenger details, travel date, booking time, and booking status. It also contains additional fields like seat number, bus number, source, destination, fare, and departure time so that one object can hold all the information needed to print a complete ticket.

So overall, the model layer defines the structure of the data flowing through the whole project. Whenever DAO retrieves records, it stores them in model objects, and then the service and main layers use these objects for processing and display.

At this point in the demo, when route details, bus details, seat details, and booking tickets are shown on the screen, all of that displayed data is being managed through these model classes.

### Person 4 Script - Service Layer and Booking Execution

I worked on the service layer, which contains the business logic of the project. In our system, we have `RouteService`, `BusService`, `SeatService`, and `BookingService`. This layer takes data from the DAO layer, applies logic, and sends the final output to the main application.

`RouteService` is responsible for displaying all routes, searching routes, and allowing the user to select a route interactively.

`BusService` displays buses for a selected route and date, shows departure time, total seats, and available seats, and helps the user select a specific bus.

`SeatService` displays the seat layout visually. It gets seat data for the selected bus and date, marks booked and available seats, and also validates whether the chosen seat can be booked. This improves user experience because users can clearly see the seat map before booking.

The most important part is `BookingService`, because it handles the actual booking process. In the `bookSeat()` method, we open a new database connection, disable auto-commit, and begin a transaction. Then we find the selected seat and check availability using row-level locking. If the seat is already booked, the transaction is rolled back. If the seat is free, a booking record is inserted and the transaction is committed. This is how our system prevents double booking.

Now I will continue the execution demo.

First, from the main menu, we choose option 4, which is book a ticket.

Step 1: We select a route, for example Delhi to Mumbai.

Step 2: We enter the travel date.

Step 3: The system shows all buses available on that route for the selected date.

Step 4: We choose one bus, and the system displays the seat map. Available seats and booked seats are shown separately.

Step 5: We enter a seat number, for example 01, and then fill in passenger name, phone number, and optional email.

Step 6: The system shows the booking summary and asks for confirmation.

After confirmation, `BookingService` processes the transaction. If successful, the system prints a ticket with booking ID, passenger name, bus number, route, seat number, travel date, departure time, fare, and status.

To show another feature, we can go back to the main menu and choose option 6 to view booking details by booking ID, or option 7 to search bookings using phone number. We can also choose option 5 to cancel the booking. After cancellation, the booking status changes to cancelled and the seat becomes available again.

So this completes the full lifecycle of our application: route selection, bus selection, seat visualization, booking confirmation, ticket generation, search, and cancellation.

### Closing Lines - Person 1

To conclude, our Bus Ticket Booking System demonstrates how Java, JDBC, and MySQL can be combined to build a real-world reservation system using a layered architecture.

This project helped us understand database connectivity, modular programming, transaction handling, row-level locking, and clean separation between model, DAO, service, and main application layers.

Thank you.

## Suggested Demo Sequence for the Video

1. Show project folder structure.
2. Show `database_setup.sql` and mention the four main tables.
3. Show `sample_data.sql` and mention sample routes, buses, and auto-generated seats.
4. Show `db.properties`.
5. Run compile command.
6. Run the application.
7. Choose `1` and show all routes.
8. Choose `4` and complete one booking.
9. Choose `6` and show booking by ID.
10. Choose `7` and search by phone number.
11. Choose `5` and cancel the booking.
12. End with architecture summary.

## Short Execution Dialogue During Demo

### While booking
- "Here we are selecting route ID 1."
- "Now we enter the travel date."
- "The system displays available buses with seat counts."
- "Now we select bus ID 1."
- "Here the seat layout is displayed."
- "We choose seat 01 and enter passenger details."
- "After confirmation, the booking is processed using a transaction."
- "Now the generated ticket is displayed."

### While cancellation
- "Now we use the generated booking ID."
- "The system fetches the booking details."
- "After confirmation, the booking is cancelled."
- "The seat becomes available again for future booking."

## Presenter Tip

Keep the speed natural and do not read too fast. If each person speaks clearly for around 2 to 3 minutes and the demo is shown during Person 1 and Person 4 sections, the total video will be close to 10 minutes.
