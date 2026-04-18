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
