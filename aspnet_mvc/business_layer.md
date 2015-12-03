# Business Layer

## Data Access Frameworks / ORMs
Data access frameworks and ORMs provide access to stored data. Examples of frameworks include
* Entity Framework
* LinqToSql (deprecated)
* AWS Dynamo DB
* Redis (ServiceStack)
* Web Services (HttpClient)
* ect.


Most of these frameworks have Data Operations and Data Representation aspects to them. Generally these are separated i.e. The entity framework has Data Models that are used in DbSets<T> which are used to perform data operations.

### Data Transfer Objects
Objects that represent data are often called Data Transfer Objects (DTOs). It is critical that these objects are used just for data access. They should not be used in other layers of the application. Failing to do this leads to tight coupling and low cohesion. Application logic, potentially even view logic gets tied directly to the data access layer. Changes to the data access will require changes to application logic and view logic and a failure to do so will break those layers. Data access needs to be encapsulated by the Modules that make up the core business logic of the application.

##Modules
Modules represent the core business logic of the application. Each module should aim to have well defined boundaries. Modules provide services to be used by other layers of the application. These services may include data access and manipulation as well as business logic.
