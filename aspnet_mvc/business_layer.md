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


## Example

``` c#
public class TodoItemDto {
    public int ID { get; set; }
    public string Title { get; set; }
    public bool Done { get; set; }
}

// Implementation using some persistance service (SQL, NoSQL, REST API)
public interface ITodoStorageService {
    IEnumerable<TodoItemDto> GetTodos();
    TodoItemDto GetTodo(int id);
    TodoItemDto CreateTodo(string title);
    void UpdateTodo(int id, string title, bool done);
}

public class TodoService {
    private readonly ITodoStorageService storage;
    
    public TodoService(ITodoStorageService storage) {
        this.storage = storage;
    }
    
    // Get the TodoDtos
    // This has the potential for DTOs to be used inappropriately
    // This can lead to duplication if multiple components access todo's in the same way
    //   i.e. getting incomplete todos
    public IEnumerable<TodoItemDto> GetTodos() {
        return this.storage.GetTodos();
    }
    
    // Restrict access to DTOs by requiring a selector
    //   This prevents the DTOs being used downstream in inappropriate places
    // Apply business logic to the data at this layer
    public IEnumerable<T> GetIncompleteTodoModels<T>(Func<TodoItemDto, T> selector) {
        return this.storage
                    .GetTodos()
                    .Where(t => !t.Done)
                    .Select(selector);
    }
    
    // Business logic to mark an items as done
    public void MarkDone(int id) {
        var todo = this.storage.GetTodo(id);
        if (todo == null) throw new Exception("Could not find that todo");
        
        this.storage.UpdateTodo(todo.ID, todo.Title, true);
    }
}

```