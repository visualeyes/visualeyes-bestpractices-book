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
