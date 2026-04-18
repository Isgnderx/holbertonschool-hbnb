# holbertonschool-hbnb

# 0 - HBnB - High Level Package Diagram

```mermaid
flowchart TB
    subgraph Presentation_Layer["Presentation Layer"]
        API["API Endpoints"]
        Services["Services"]
    end

    subgraph Business_Logic_Layer["Business Logic Layer"]
        Facade["HBnB Facade"]
        User["User"]
        Place["Place"]
        Review["Review"]
        Amenity["Amenity"]
    end

    subgraph Persistence_Layer["Persistence Layer"]
        Repository["Repositories / DAO"]
        Database["Database"]
    end

    API --> Services
    Services --> Facade

    Facade --> User
    Facade --> Place
    Facade --> Review
    Facade --> Amenity

    User --> Repository
    Place --> Repository
    Review --> Repository
    Amenity --> Repository

    Repository --> Database
```

```markdown
## Explanation

- **Presentation Layer** handles user interaction through API and services.
- **Business Logic Layer** contains core models like User, Place, Review, and Amenity.
- **Persistence Layer** manages data storage and database operations.
- **Facade Pattern** simplifies communication between layers by providing a unified interface.
```

# 1 - HBnB - Detailed Class Diagram

```mermaid
classDiagram

class BaseModel {
    +UUID4 id
    +datetime created_at
    +datetime updated_at
    +save()
    +update()
    +delete()
    +to_dict()
}

class User {
    +string first_name
    +string last_name
    +string email
    +string password
    +register()
    +update_profile()
}

class Place {
    +string title
    +string description
    +float price
    +float latitude
    +float longitude
    +create_place()
    +update_place()
}

class Review {
    +string text
    +int rating
    +submit_review()
    +update_review()
}

class Amenity {
    +string name
    +create_amenity()
    +update_amenity()
}

User --|> BaseModel
Place --|> BaseModel
Review --|> BaseModel
Amenity --|> BaseModel

User "1" --> "0..*" Place : owns
User "1" --> "0..*" Review : writes
Place "1" --> "0..*" Review : has
Place "*" --> "*" Amenity : includes
```

```markdown
## Explanation

- BaseModel
- User
- Place
- Review
- Amenity
- inheritance
- associations
- multiplicity
```

## 2. Sequence Diagrams for API Calls

This section presents four sequence diagrams that illustrate how different layers of the HBnB application interact to process API requests.  
Each diagram shows the communication flow between the **Presentation Layer**, **Business Logic Layer**, and **Persistence Layer**.

---

### 2.1 User Registration

```mermaid
sequenceDiagram
    actor User
    participant API as API Endpoint
    participant Facade as HBnB Facade
    participant UserModel as User Model
    participant Repository as User Repository
    participant DB as Database

    User->>API: POST /users/register
    API->>Facade: register_user(user_data)
    Facade->>UserModel: validate and create user
    UserModel-->>Facade: user instance
    Facade->>Repository: save(user)
    Repository->>DB: INSERT user data
    DB-->>Repository: success
    Repository-->>Facade: user saved
    Facade-->>API: return success response
    API-->>User: 201 Created
```

```markdown
##Explanation

This sequence diagram shows how a new user account is created in the system.

The user sends a registration request to the API.
The API forwards the request to the facade.
The facade validates the input and creates a new User object.
The user data is then saved through the repository into the database.
Finally, a success response is returned to the client.
```

### 2.2 Place Creation

```mermaid
sequenceDiagram
    actor User
    participant API as API Endpoint
    participant Facade as HBnB Facade
    participant PlaceModel as Place Model
    participant Repository as Place Repository
    participant DB as Database

    User->>API: POST /places
    API->>Facade: create_place(place_data)
    Facade->>PlaceModel: validate and create place
    PlaceModel-->>Facade: place instance
    Facade->>Repository: save(place)
    Repository->>DB: INSERT place data
    DB-->>Repository: success
    Repository-->>Facade: place saved
    Facade-->>API: return success response
    API-->>User: 201 Created
```

```markdown
##Explanation

This diagram represents the process of creating a new place listing.

The user submits place data through the API.
The API passes the request to the facade.
The facade applies business rules and creates a Place object.
The place is persisted using the repository and stored in the database.
The API returns a confirmation response.
```

### 2.3 Review Submission

```mermaid
sequenceDiagram
    actor User
    participant API as API Endpoint
    participant Facade as HBnB Facade
    participant ReviewModel as Review Model
    participant Repository as Review Repository
    participant DB as Database

    User->>API: POST /reviews
    API->>Facade: submit_review(review_data)
    Facade->>ReviewModel: validate and create review
    ReviewModel-->>Facade: review instance
    Facade->>Repository: save(review)
    Repository->>DB: INSERT review data
    DB-->>Repository: success
    Repository-->>Facade: review saved
    Facade-->>API: return success response
    API-->>User: 201 Created
```

```markdown
##Explanation

This sequence diagram shows how a user submits a review for a place.

The user sends review information to the API.
The API forwards the request to the facade.
The facade validates the request and creates a Review object.
The review is stored in the database through the repository.
A success response is then returned.
```

### 2.4 Fetching a List of Places

```mermaid
sequenceDiagram
    actor User
    participant API as API Endpoint
    participant Facade as HBnB Facade
    participant Repository as Place Repository
    participant DB as Database

    User->>API: GET /places?filters=criteria
    API->>Facade: get_places(criteria)
    Facade->>Repository: find_all(criteria)
    Repository->>DB: SELECT places by criteria
    DB-->>Repository: places data
    Repository-->>Facade: list of places
    Facade-->>API: formatted results
    API-->>User: 200 OK with places list
```

```markdown
##Explanation

This diagram illustrates how the system retrieves a list of places based on user-defined criteria.

The user sends a request to fetch places.
The API sends the request to the facade.
The facade delegates the search to the repository.
The repository queries the database and returns matching results.
The response is formatted and sent back to the user.
```
