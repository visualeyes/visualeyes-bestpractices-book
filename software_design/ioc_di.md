# Inversion of Control and Dependency Injection

Inversion of control and dependency injection are powerful tools that can help reduce coupling.

Inversion of control is a design pattern that aims to reduce awareness of implementations. It reduces coupling as by ensuring codes are dependent on an interface contract rather than a single implementation of that contract.

If our original console printer example had to get text from a database we can use an interface rather than the implementation to reduce coupling (some loose coupling still exists)

```csharp
public interface ITextService {
  string GetText();
}

public class DatabaseTextService {
  ...
  public string GetText() {
    return db.Text.First(t => t.Text);
  }
}

public class ConsolePrinter {
  private readonly ITextService textService;

  public ConsolePrinter(ITextService textService) {
    this.textService = textService;
  }

  public void WriteText() {
    Console.WriteLine(this.textService.GetText());
  }
}

new ConsolePrinter(new DatabaseTextService()).WriteText();
```

How is this better?
ConsolePrinter is dependent on the interface ITextGetter not the implementation. If we need to get text from a web service and/or database this can be easily changed in a few places rather than changing all references to ConsolePrinter. 

How does Dependency Injection further reduce coupling of components?
Dependency Injection enables us to have a DI Container construct objects at runtime injecting the services into our classes. This enables us to specify implementations of a service in a reduced number of places rather than throughout the entire code base. 

```csharp
public interface ITextService {
  string GetText();
}

public class DatabaseTextService {
  ...
  public string GetText() {
    return db.Text.First(t => t.Text);
  }
}

public class WebSerivceTextService {
  ...
  public string GetText() {
    return new WebService().RequestText();
  }
}

public class ConsolePrinter {
  private readonly ITextService textService;

  public ConsolePrinter(ITextService textService) {
    this.textService = textService;
  }

  public void WriteText() {
    Console.WriteLine(this.textService.GetText());
  }
}

var container = new DIContainer(); // Unity, Autofac ect.
container.RegisterType<ITextService, DatabaseTextService>();

// Note, resolving like this is generally not a good idea
// Containers should be used in very limited number of places
var printer = container.Resolve<ConsolePrinter>();
printer.WriteText();
```

DatabaseTextService is registered once resolved many times throughout the application.

Service locator
The service locator pattern is when you manually resolve Service inside constructors or methods.

```csharp
public class MyType {
  public void MyMethod() {
   var dep1 = Locator.Resolve<IDep1>();
   dep1.DoSomething(); // new dependency
   var dep2 = Locator.Resolve<IDep2>();
   dep2.DoSomething(); 
  } 
}
```

This hides dependencies which makes it more difficult to maintain. Your method is dependent on the service locator and the internal dependencies. This can make testability more difficult (or impossible) if the Locator cannot be accessed.

Generally the frameworks handle the service location aspect i.e. ASP.NET MVC and DI containers use service location to create your Controllers or Objects internally. You should leave Service Location to those frameworks and not use this pattern yourself.

## Examples

**Simple Dependency Resolution**

```csharp
public interface ITextService {
  string GetText();
}

public class ConsolePrinter {
  private readonly ITextService textService;

  public ConsolePrinter(ITextService textService) {
    this.textService = textService;
  }

  public void WriteText() {
    Console.WriteLine(this.textService.GetText());
  }
}
```

**Lazy Dependency Resolution**

```csharp
public interface ITextService {
  string GetText();
}

public class ConsolePrinter {
  private readonly Lazy<ITextService> lazyTextService;

  public ConsolePrinter(Lazy<ITextService> lazyTextService) {
    this.textlazyTextServiceService = lazyTextService;
  }

  public void WriteText() {
    var textService = lazyTextService.Value; // Created now
    Console.WriteLine(textService.GetText());
  }
}
```
This doesn’t create a text service until it’s required. Useful for Network Services i.e. database.

**Dependency Auto Factory**
```csharp
public interface ITextService {
  string GetText();
}

public class ConsolePrinter {
  private readonly Func<ITextService> textServiceFactory;

  public ConsolePrinter(Func<ITextService> textServiceFactory) {
    this.textServiceFactory = textServiceFactory;
  }

  public void WriteText() {
    var textService = textServiceFactory(); // Created now
    Console.WriteLine(textService.GetText());
  }
}
```

This creates a factory that can create Text Services as needed (note you can also create factories that take constructors).