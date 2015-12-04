# Testing
Testing is a critical part of delivering high quality code. Testing helps prevent regressions and bugs. 

## Unit Tests
Unit tests Assert that the logic in components operate as expected. Each Unit test should isolate a small part of a component and test that it operates as expected. Each test should be independent of other tests. Unit tests should not access persisted state or network services. This includes:

* Relational Databases
* NoSql Databases
* Files
* Web Services


### Facts and Theories (xUnit)

**Fact Example**
``` c#
[Fact]
public void One_Equal_One() {
    Assert.Equal(1, 1);
}
```

**Theory Example**
``` c#
[Theory]
[InlineData(1, 2)]
[InlineData(2, 4)]
public void Num_Times_Two(int num, int expected) {
    int actual = multiplier.MultiplyByTwo(num);
    Assert.Equal(expected, actual);
}
```

### Mocking
Mocking in unit tests isolates the code being tested from it's dependencies. Using mocks dependencies can be replaced with an alternative implementation that can be controlled.

There are three main techniques used to acheive this:
* **Fakes:** Implements the dependency and return fixed results
* **Stubs:** Implements the dependency and returns pre-recorded results (set in the test)
* **Mocks:** Implements the dependency, returns pre-recorded results and records how dependencies are used

Mocks are the best solution for isolating code. [Moq](http://www.moqthis.com/) is the Mocking framework we recommend.

**Example using Moq**
``` c#
[Fact]
public void Get_Todos() {
    var mockStorage = new Mock<ITodoStorageService>();
    var todoService = new TodoService(mockStorage.Object);
    
    mockStorage
      .Setup(s => s.GetTodos())
      .Returns(new List<TodoItemDTO>() {
        new TodoItemDto() { Title = "Todo", Done = false },
        new TodoItemDto() { Title = "Done", Done = true }
      });
      
    
    var todos = todoService.GetIncompleteTodoModels(t => t);
    Assert.Equal(1, todos.Count());
}
```

In this example we ensure the todo service gets todos and only returns incomplete todos.

## Component Testing
Component Testing tests a single component or a module. This is a broader test than a unit test as it tests the component and it's dependencies.

**Example**
``` c#
[Fact]
public void Get_Todos() {
    var todoStorage = new MemoryTodoStorageService(); // memory based service 
    var todoService = new TodoService(todoStorage);
    var todoController = new TodoController(todoService);
    
    var viewResult = todoController.List();
    
    Assert.NotNull(viewResult);
    Assert.NotNull(viewResult.Model);
}
```

In this example we ensure that the TodoController will create the a view and send a model to the view.

## Integration Tests
Integration tests combine modules and dependencies to test the system as a whole. These tests verify functionality, performance and reliability of a whole module.

### State
Integration tests may interact with state and network resources. It is critical to ensure that these tests start with an expected state. Failing to do this will result in inconsistent tests results. These tests should be limited and clean up any modified state after they complete.

**Example**
``` c#
[Fact]
public void Get_Todos() {
    var todoStorage = new TodoStorageService();
    var todoService = new TodoService(todoStorage);
    var todoController = new TodoController(todoService);
    
    var viewResult = todoController.List();
    
    Assert.NotNull(viewResult);
    Assert.NotNull(viewResult.Model);
}
```

This example extends the previous example by attempting to retrieve todos from a database.

## Performance Tests
Performance tests measure the performance of a component and verify that the result is an acceptable range.

## Code Coverage
* Open Cover
 

## Reading
* http://martinfowler.com/articles/microservice-testing/