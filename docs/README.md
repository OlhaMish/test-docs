## Use cases

### Student Sequence Diagrams

#### Student Registration
@startuml
|Student|
start
:Goes to the registration page;
:Indicates the type of user - Student;
:Enters registration data \n(name, email, password, phone number);

|System|
:Checks for correctness\nentered data;
note right #ff8c69
Possible USER.REGISTRATION_ERROR
end note
:Creates a new account for the user;

|Student|
:Receives confirmation\nabout successful registration;
stop
@enduml


#### Student Login 
@startuml
|Student|
start;
:Goes to the authorization page;
:Enter your credentials;

|System|
:Checks for such an account;

note right #ff8c69
Possible USER.LOGIN_ERROR;
end note

:Provides access to the \nstudent's personal account;

|Student|
:Logs in to the site using an account;
 stop;
@enduml


#### Student Search Tutor 
@startuml

|Student|
start;
:Starts interaction;
:Uses filters for query;
:Clicks on the "Search" button;

|System|
:Performs a search at the request\nof the user in the database;

note right #ff8c69
Possible USER.DATA_SEARCH_ERROR;
end note

:Displays tutors that satisfy the\nsearch query in the form of a list;

|Student|
:Ends interaction;
stop;
@enduml


#### Student Contact Tutor
@startuml
|User|
start
:Goes to the tutor's page;
:Views information about the tutor;
:Presses the "Contact Tutor" button;

|System|
:Checks if the user is registered;

note right #ff8c69
Possible USER.NOT_LOGGED_IN_ERROR;
end note

:Opens a chat for\ncommunication with the tutor;

|User|
:Starts chat;

|System|
:Sends a message to the tutor;

|Tutor|
:Receives chat messages;

|User|
:Communicates with the tutor via chat;

stop
@enduml


#### Student Registration
```mermaid
sequenceDiagram
    participant User
    participant ClientApp
    participant AuthService
    participant Database

    User->>ClientApp: Fill Registration Form
    ClientApp->>AuthService: Submit Registration Data
    AuthService->>Database: Check if User Exists
    alt User Exists
        AuthService->>ClientApp: HTTP 400 Bad Request (User Already Registered)
    else New User
        AuthService->>Database: Save New User Data
        Database-->>AuthService: User Saved
        AuthService->>ClientApp: HTTP 201 Created (Registration Successful)
    end
    ClientApp->>User: Display Registration Status
```

#### Student Login
```mermaid
sequenceDiagram
    participant User
    participant ClientApp
    participant AuthService
    participant Database

    User->>ClientApp: Enter Username & Password
    ClientApp->>AuthService: Submit Login Credentials
    AuthService->>Database: Retrieve User Data
    alt Valid Credentials
        AuthService->>AuthService: Generate Session Token
        AuthService->>ClientApp: Login Successful (Session Token)
    else Invalid Credentials
        AuthService->>ClientApp: Login Failed
    end
    ClientApp->>User: Display Login Status
```


```mermaid
sequenceDiagram
    participant Student
    participant ClientApp
    participant System
    participant Database

    Student->>ClientApp: Uses filters and clicks "Search" button
    ClientApp->>System: Submit search request
    System->>Database: Perform search query
    alt Tutors Found
        System->>ClientApp: Display list of tutors
    else Data Search Error
        System->>ClientApp: USER.DATA_SEARCH_ERROR
    end
    ClientApp->>Student: Display search results or error message
```


```mermaid
sequenceDiagram
    participant User
    participant ClientApp
    participant System
    participant Tutor

    User->>ClientApp: Access tutor's page and click "Contact Tutor"
    ClientApp->>System: Request to open chat
    System->>System: Check if user is registered
    alt User Not Logged In
        System->>ClientApp: USER.NOT_LOGGED_IN_ERROR
    else User Logged In
        System->>ClientApp: Open chat window
        ClientApp->>User: Chat ready for communication
        User->>System: Send message to tutor
        System->>Tutor: Deliver message
        Tutor->>User: Tutor replies to message
    end
```