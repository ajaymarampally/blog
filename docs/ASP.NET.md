# ASP.NET

Course Link [Udemy- Mosh Hamedani](https://gale.udemy.com/course/the-complete-aspnet-mvc-5-course/learn/lecture/4847008#overview)

## MVC

- Model : Representation of a data object used in other classes.
- View : Layer responsible for transformation of information or data from model and present it in UI
- Contoller: Layer responsible for handling all the requests. It interacts with the `Model` to fetch data and `View` to make changes to display in the UI.
- Router : The layer which is responsible for routing the requests to the appropriate controller in the backend.

## Return Types

- ActionResult - whenever a action is invoked in a controller, some specific logic is performed and an `ActionResult` object is returned, which indicates what the server should respond to the request
- ViewResult: This is the most common type, used to render a view template (typically a Razor view) for the client.
- ContentResult: This returns plain text content directly to the client.
- EmptyResult: This indicates no specific content needs to be returned, often used for simple actions that might just perform some processing.
- RedirectResult: This redirects the user's request to a different URL.
- JsonResult: This returns data in JSON format, often used for AJAX requests in web applications.
- JavaScriptResult: This returns a JavaScript code snippet that can be executed on the client-side.

