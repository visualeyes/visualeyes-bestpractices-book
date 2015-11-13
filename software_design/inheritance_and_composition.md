# Inheritance and Composition

This can be a contentious issue. It is easy to become stuck in thinking the best approach is always either inheritance or composition. Both have important uses and should be used together appropriately. This article summarises the issue very well and is a must read.

**When to use inheritance:**
* Both classes are in the same logical domain
* The subclass is a proper subtype of the superclass
* The superclass’s implementation is necessary or appropriate for the subclass
* The enhancements made by the subclass are primarily additive.

**When not to use inheritance:**
* The hierarchy is very large
* Logic in the superclass is independent of logic in the subclass

**Example of Good Inheritance**
```c#
public abstract class Widget {
  public virtual string GetClassName() {
    return "widget";
  }

  public abstract string GetBody();

  public string ToHtml() {
    string className = this.GetClassName();
    string body = this.GetBody();
    return String.Format("<div class=\"{0}\">{1}</div>", className, body);
  }
}

public sealed class BlankWidget : Widget {
  public override string GetClassName() {
    // this is somewhat questionable i.e. do all implementations override widget
    return "blank-widget";
  }

  public override string GetBody() {
    return "";
  }
}

public class WidgetRenderer {
  public string Render(Widget widget) {
    return "<html><body>" + widget.ToHtml() + "</body></html>";
  }
}

```


**Why is this ok?**
* Blank Widget is a Widget -> they have the same domain
* Widget implementation is appropriate for Blank Widget
* Blank widget only adds functionality
* It is sealed at one level
* In terms of coupling WidgetRenderer is loosely coupled to Widget  but BlankWidget is tightly coupled to Widget as it is dependent 
* 

**Example of Bad Inheritance**
```c#
public abstract class Widget {
  public virtual string GetClassName() {
    return "widget";
  }

  public abstract string GetBody();

  public string ToHtml() {
    string className = this.GetClassName();
    string body = this.GetBody();
    return String.Format("<div class=\"{0}\">{1}</div>", className, body);
  }
}

public class SectionRenderer : Widget {
  private readonly string html;
  public SectionRenderer (string html) {
    this.html = html;
  }

  public override string GetClassName() {
    return "";
  }

  public override string GetBody() {
    return this.html;
  }
}

public class WidgetRenderer {
  public string Render(Widget widget) {
    return "<html><body>" + widget.ToHtml() + "</body></html>";
  }
}
```
**Why is this not ok?**
* Section Renderer is rendering arbitrary html, not widgets -> they do not have the same domain
* Widget implementation is not appropriate for Section Renderer

**Example of with Composition + Inheritance from Interfaces**
```c#
public interface IWidget {
  string GetClassName();
  string GetBody();
}

public sealed class BlankWidget : IWidget {
  public string GetClassName() {
    return "blank-widget";
  }

  public string GetBody() {
    return "";
  }
}

public class WidgetRenderer {

  public string Render(IWidget widget) {
    string className = widget.GetClassName();
    string body = widget.GetBody();
    return String.Format(
      "<html><body><div class=\"{0}\">{1}</div></body></html>", 
      className, 
      body
    );
  }
}

```

Why is this ok?
* Blank Widget is an IWidget -> they have the same domain
* In terms of coupling Blank widget is loosely coupled to widget renderer.
