# Components
Component based software development emphasizes the separation of concerns. Software projects contain small components that interact with each other. Each component should address a specific concern (high cohesion) and be independent of other components (loose coupling).

### What does this mean and why is it important?
The purpose of this it to think of the piece of software in smaller and smaller parts. For example, a feature is a component and there are components within the feature. Each component in the feature works together to deliver the feature as a whole.

## Component Boundaries
The critical part of creating components is well defined boundaries for components. To create well defined boundaries, components should aim to have high cohesion and be loosely coupled.
* Components with high cohesion focus on one task. 
* Components with loose coupling have limited dependencies on other components. 

High cohesion makes components easier to understand as they focus on a specific task. Loose coupling makes code easier to change and maintain. Changing tightly coupled components results in ripple effects thought an application. For example, changing x requires a change to y followed by changes to z and so on.

**Example of Tight Coupling - Console Printer**
```csharp
public class TextService {
  public string GetText() {  return "text";  }
}

public class ConsolePrinter {
  private readonly TextGetter textService;
  public ConsolePrinter() {
    this.textService = new TextService();
  }
  public void WriteText() {
    Console.WriteLine(this.textService.GetText());
  }
}

new ConsolePrinter().WriteText();
```

Console printer is tightly coupled to TextService as it requires TextService to be able to WriteText.

**Example of with No Coupling - Console Printer**
``` csharp
public class TextService {
  public string GetText() { return "text";  }
}

public class ConsolePrinter {
  public void WriteText(string text) {
    Console.WriteLine(text);
  }
}

string text = new TextService.GetText();
new ConsolePrinter().WriteText(text);
```

ConsolePrinter and TextService are not coupled as each operates independently of each other.

**Example with loose Coupling - Console Printer**
```c#
public interface ITextService {
  string GetText();
}

public class TextService {
  public string GetText() {
    return “text”;
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

ConsolePrinter is Loosely coupled to ITextService and not coupled to TextService at all.


