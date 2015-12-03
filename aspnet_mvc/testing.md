# Testing
## Unit Tests
* Small
* Isolated
* No Network Resources (DB, Web Services)

### Facts and Theories (xUnit)

``` c#
[Fact]
public void One_Equal_One() {
    Assert.Equal(1, 1);
}
```

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


## Integration Tests
* Test components together
* Can test against network resources

### Data Driven Tests
* Start with an expected state
* Limit what you test
* Cleanup
