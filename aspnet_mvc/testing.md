# Testing
Testing is a critical part of delivering high quality code. Testing helps prevent regressions and bugs.

## Unit Tests
Unit tests Assert that the logic in components operate as expected. Each Unit test should isolate a small part of a component and test that it operates as expected. Each test should be independent of other tests. Unit tests should not access persisted state or network services. This includes:
* Relational Databases
* NoSql Databaes
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
public void Num_Times_Two(int num, int expected) {
    var mockStorage = new Mock<ITodoStorageService>();
    var todoService = new TodoService(mockStorage.Object);
    
    mockStorage
      .Setup(s => s.GetTodos())
      .Returns(new List<TodoItemDTO>());
      
    
    var todos = todoService.GetTodos();
    Assert.Empty(todos);
}
```

### Code Coverage
* Open Cover

## Integration Tests
* Test components together
* Can test against network resources

### Data Driven Tests
* Start with an expected state
* Limit what you test
* Cleanup

## Performance Tests
