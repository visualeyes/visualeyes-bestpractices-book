# Web Layer
The Web Layer provides interfaces to the Users. The interfaces in the web layer can come in different forms. For example, a HTML View, Single Page Application, RESTful API or RSS feed.

## Areas
Areas represent a grouping of Models, Views, Controllers + Framework code. Smaller applications might choose to not use Areas and simply use these components in the root of the Application.

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
```c#
public class Controller {
    [Route("/todos"), HttpGet]
    public ActionResult List() {
        return View();
    }
    
    [Route("/todos/markdone/{id}"), HttpPost]
    public ActionResult MarkDone(int id) {
    }
}
```

## Views
* Avoid doing complicated stuff with ViewBag
* Can take advantage of HTML Helpers
* However no restrictions against writing your own html
* If you want to write your own html @Html.NameFor

## Framework
