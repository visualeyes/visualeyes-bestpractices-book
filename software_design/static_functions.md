# Static Functions

Static methods need to be used with caution. Public static methods are inherently tightly coupled to the components that use them. Public static methods also present a problem for testability.

```csharp
// Ignore that we're not doing any locking
public static class ItemCache {
  private static IEnumerable<Item> items;
  
  public static IEnumerable<Item> GetItems() {
    EnsureLoaded();
    return items;
  }

  public static void Reload() {
    items = // get items from db somehow;
  }
  public static void EnsureLoaded() {
    if(items == null) Reload();
  }
}

public class ItemUser {
  public Item GetItem(int id) {
    var items = ItemCache.GetItems();
    return items.SingleOrDefault(item => item.ID == id);
  }
}
```

## What is wrong with this?
ItemUser is tightly coupled with ItemCache. Any place that uses a method on ItemCache will be tightly coupled to it. If we need to change ItemCache, we will end up needing to make a large number of changes. Furthermore, we cannot properly test ItemUser as it will make calls to the database.

## When to use static methods?
Static methods can be useful when they are stateless and do not create any internal dependencies.

```csharp
public static ListTypeHelper {
  // Good
  public static bool IsList<T>(object item) {
    return item is List<T>;
  }

  // Good
  public static bool IsEnumerable(this object item) {
    return item is IEnumerable;
  }

  // bad - it is tightly coupled to ListToHashSetConverter
  public static HashSet<T> ConvertToHashSet<T>(List<T> list) {
    var converter = new ListToHashSetConverter();
    return converter.Convert(list);
  }
}

var list = new List<object>();
ListTypeHelper.IsList<object>(list);
list.IsEnumerable();
```