# Web Layer
The Web Layer provides interfaces to the Users. The interfaces in the web layer can come in different forms. For example, a HTML View, Single Page Application, RESTful API or RSS feed.

## Areas
Areas represent a grouping of Models, Views, Controllers + Framework code.

## Models
Models represents data sent from the client and to the client through views. This dual role can be split into RequestModels and ViewModels if required. As stated earlier it is very important not to confuse Data Transfer Objects (DTOs) with models.

Request models are responsible for holding the data sent to the WebServer from the client. These models are can should provide validation logic using Validation Attributes or the `IValidatableObject` interface.

View Models are responsible for holding data s

* View Models -> not DTOs (Data Transfer Objects)
* Validate in models

## Controllers
* Dumb controllers. Inject services that perform the complex logic you require
  * i.e. storage services


How to handle validation that requires validating outside of the model

## Views
* Avoid doing complicated stuff with ViewBag
* Can take advantage of HTML Helpers
* However no restrictions against writing your own html
* If you want to write your own html @Html.NameFor

## Framework
