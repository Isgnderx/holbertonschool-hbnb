# holbertonschool-hbnb

# HBnB - High Level Package Diagram

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
