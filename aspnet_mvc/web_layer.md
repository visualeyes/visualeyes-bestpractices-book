# Web Layer
The Web Layer provides interfaces to the Users. The interfaces in the web layer can come in different forms. For example, a HTML View, Single Page Application, RESTful API or RSS feed.

## Areas
Areas represent a grouping of Models, Views, Controllers + Framework code. Smaller applications might choose to not use Areas and simply use these components in the root of the Application. The Framework code is not standard in MVC, we have added it to provide a place for Services specific to an Area or the Web Layer.

## Models
Models represents data sent from the client and to the client through views. This dual role can be split into RequestModels and ViewModels if required. Data Transfer Objects (DTOs) should not be used as models. Doing so increases coupling and reduces cohesion.

Request models are responsible for holding the data sent to the WebServer from the client. These models are can should provide validation logic using Validation Attributes or the `IValidatableObject` interface.

View Models are responsible for holding the data sent to the View. These models should only contain data transformation logic. This logic should only exist to convert data into appropriate formats for the View.

**Examples**
``` c#
public class AddTodoRequestModel {
    [Required]
    public string Title { get; set; }
}

public class TodoViewModel {
    public string Title { get; set; }
    public DateTime AddedUtc { get; set; }
    
    public string GetAddedDateString() {
        // convert to local time with friendly format
        return "today";
    }
}
```

## Controllers
Controllers handle User Actions and returns a Model and a View to the User. Controllers are the junction between the Web Layer and Business Layer. Controllers should not perform complex logic. Instead they utilise services in the Business Layer to perform complex operations based on User actions.


How to handle validation that requires validating outside of the model

**Example**
``` c#
public class TodoController {
    private readonly ITodoService todoService;
    
    public TodoController(ITodoService todoService) {
        this.todoService = todoService;
    }
    
    [Route("/todos"), HttpGet]
    public ActionResult List() {
        var models = todoService.GetIncompleteTodoModels(t => new TodoListItemModel {
            Title = t.Title
        });
        
        return View(models);
    }
    
    [Route("/todos/markdone/{id}"), HttpPost]
    public ActionResult MarkDone(int id) {
        todoService.MarkDone(id);
        return RedirectToAction("list");
    }
}
```

## Views
Views generate HTML to present to the user based on a Model.

ASP.NET MVC provide a `ViewBag` in the View and Controller. This is a dynamic object that can be used the send data from the Controller to the View. The ViewBag should be used to replace a Model. It should only be used in a limited way where a Model cannot be used. An example of this may be setting the current User's name on the ViewBag from an Attribute.

ASP.NET MVC provies HTML Helpers that generate HTML. These Helpers are useful for generating Html for Properties on a Model i.e. `Html.InputFor(t => t.Title)`. However, this does not restrict you from writing HTML in the view if required. When writing Html for properties on a Model it is advisable to use the `Html.NameFor` method.

**For example**
``` Html
<input 
  class="my-nice-input" 
  type="text" 
  name="@Html.NameFor(t => t.Title)" 
  placeholder="What do you want to do?" />
```

## Framework
The Framework component contains Services that are specific to the Web Layer or Area. These Services are primarily used by Controllers. 

For Examples:
* Identity Services
* Logging
* Caching

