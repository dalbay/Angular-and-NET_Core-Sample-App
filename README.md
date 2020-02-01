# Angular and ASP.NET Core Sample App

Tutorial on how to build an Angular - ASP.NET Core web application

**Application Overview:**

- build service layer with the ASP.NET Web API for the back end, to expose the required endpoints to create, read, update, and delete entries
- build a UI using Angular with Bootstrap;
- implement NgRx—a framework for building reactive applications in Angular—in your app;
- implement user authentication with Auth0.  

<br/>  

## A. Setting Up the App Infrastructure

### 1. .NET Core Templates

- Type in the terminal `$ dotnet new angular` to build a new angular project
- Run the project - `dotnet run` and click on the localhost link; this will open the build in template.

### 2. Web API architectural overview

- *ClientApp* folder has the Angular-related files
- *Controllers* folder has all the Web API controllers
- *Pages* folder; inside this folder we have three files, and the most important one is the *ViewImports.cshtml*, which is used to import all the necessary libraries that we can use throughout the views.  
For example the TagHelpers:
```
@using Augusta_Tech___Rural_Sourcing
@namespace Augusta_Tech___Rural_Sourcing.Pages
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
```
- *wwwroot* folder is known as the root folder for the .NET Core apps. And this folder is mainly used to store static files like images, documents...
- *Program.cs* file is the entry file for the .NET apps. In here we can see the ```Main``` method, and inside the Main method we have the ```CreateWebHostBuilder``` method. And inside this method we can see that we have configured as our startup file, the Startup.cs file. 
```C#
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateHostBuilder(args).Build().Run();
        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });
    }
```
- *Startup.cs* file which is also known as the configuration file; like the database connection strings, the services that we want to use,...  
Inside the Startup.cs we have two methods, the ```ConfigureServices``` and the ```Configure``` methods. The ConfigureSevices method is used to configure dependency interaction; the Configure method which is used to setup middle wares, routing rules,... So for example if we want to use a service in the future, we can configure it inside the ConfigureServices method.  

### 3. Angular Architectural Overview

- Angular files that were created inside the **ClientApp folder** by default:  
  1. **e2e** -  used for unit testing related code
  2. **node_modules** - libraries that we need to use to run our Angular app
  3. **src** - *all the code goes in here;* inside the source folder:  
     - **app** - Inside this folder we define the modules.  
	   - Here we have all components like **counter**, **fetch-data**,...
	   - **app.component.html** entry point file in Angular applications. If we open this file we're going to see in here that we have defined the menu and the body of our application.  
	   ```C#
	   <body>
		  <app-nav-menu></app-nav-menu>
		  <div class="container">
			<router-outlet></router-outlet>
		  </div>
	   </body>
	   ```  
	   - **app.module.ts** - configuration file in Angular projects; here we define all the components that we want to use, we define all the modules that we want to use and also the router for our Angular app.  
	   - **asset** folder - keep the static files like for example the icon, images, documents... 
	   - **environments** -  we have the two environment files like the production environment and the development environment where we define all the specific information that we want to use for each environments.  
	   - **index.html** - where we define the route component where in this case we have defined the app-route.  
	   ```HTML
	   <!DOCTYPE html>
		<html lang="en">
		  <head>
			<meta charset="utf-8" />
			<title>Augusta_Tech___Rural_Sourcing</title>
			<base href="/" />

			<meta name="viewport" content="width=device-width, initial-scale=1" />
			<link rel="icon" type="image/x-icon" href="favicon.ico" />
		  </head>
		  <body>
			<app-root>Loading...</app-root>
		  </body>
		</html>
	   ```  
	   - **angular.json** - define the Angular related configurations like for example where do we get the styles from? Which is the assets folder?...  
	   - **package.json** - define scripts like for example the script to build the Angular app which is the ng build, the script to run the test and even the dependencies like Angular animations, common, compiler...  

<br/>  

## B. Setting Up Web API

### 1. Creating Data Models  

- Data Models are used as signature for the way we want the database tables to look.  
- So, its model field will be translated into a database table field.  
- create a new folder; Data - all data related files like models, services,...
  - inside the Data folder, create another folder; Models
  - create new file Book.cs - C# file which is going to represent our books model.
	- define in here the namespace
	- write in your public class, the name is Book and then inside here we can define all the properties. 
	- optional fields - we don't always require the user to provide the value. To make this fields nullable, we just write the question mark after the datatype
	```C#
	using System;
	namespace Augusta_Tech___Rural_Sourcing.Data{
		public class Book
		{
			public int Id { get; set; }
			public string Title { get; set; }
			public string Author { get; set; }
			public string Description { get; set; }
			public double? Rate { get; set; }
			public DateTime? DateStart { get; set; }
			public DateTime? DateRead { get; set; }

		}
	} 
	```  
<br/>  


### 2. Adding a Service and Data
- here we use a static file from which we will get the data to work with. 
- create service, and configuring it. 
- Inside the Data folder, create folder Services  
  - inside here create interface - IBookService.cs. 
  ```C#
	using System.Collections.Generic;

	namespace Augusta_Tech___Rural_Sourcing.Data{
		public interface IBookService
		{
			List<Book> GetAllBooks();
			Book GetBookById(int id);
			void UpdateBook(int id, Book newBook);
			void DeleteBook(int id);
			void AddBook(Book newBook);
		}
	}
  ```
  - create service - BookService.cs.  
  ```C#
	using System.Collections.Generic;

	namespace Augusta_Tech___Rural_Sourcing.Data{
		public class BookService : IBookService
		{
			public void AddBook(Book newBook)
			{
				throw new System.NotImplementedException();
			}

			public void DeleteBook(int id)
			{
				throw new System.NotImplementedException();
			}

			public List<Book> GetAllBooks()
			{
				throw new System.NotImplementedException();
			}

			public Book GetBookById(int id)
			{
				throw new System.NotImplementedException();
			}

			public void UpdateBook(int id, Book newBook)
			{
				throw new System.NotImplementedException();
			}
		}
	}
  ```  
  - go to the startup.cs file and configure this service
    - inside the ```ConfigureServices()``` method, just after the ```AddSpaSaticFiles```, add ```services.AddTransient<IBookService, BookService>();```, which means that we are going to create a new reference to our service each time we use it in a different controller. So, which is the file that we want to inject in our controllers, that is going to be the IBookService and which is the implementation of this file, the implementation is the bookService.
  - create a static file which is a list of books.  
    - go to the data folder; inside here create a file - Data.cs. 
	  Inside here, we are going to have static class which is used to return data to our users. 
	  ```C#
		using System;
		using System.Collections.Generic;

		namespace Augusta_Tech___Rural_Sourcing.Data
		{
			public static class Data
			{
				public static List<Book> Books => allBooks;

				private static List<Book> allBooks = new List<Book>()
				{
					new Book()
					{
						Id=1,
						Title="Managing Oneself",
						Description="We live in an age of unprecedented opportunity: with ambition, drive, and talent, you can rise to the top of your chosen profession, regardless of where you started out...",
						Author= "Peter Ducker",
						Rate= (double)4.9,
						DateStart = new DateTime(2019,01,20),
						DateRead = null
					},
					new Book()
					{
						. . . 
					}
				};
			}
		}
	  ```  
<br/>  

### 3. Create API Endpoints 

- Create a **Controllers** folder which will have the **API Endpoints**. 
- Inside the Controllers folder and create a new Controller file - *BooksController.cs*
  - Define the namespace and inside here create all book controllers
  - Define the route for this controller - ```[Route("api/[controller]")]``` 
  - Inherit ```Controller``` base class, for a class to be a controller which is the AspNetCore.mvc
  - Create a field to use BookService with ```IBookService``` and Inject BookService in our constructor. 
  - To create the constructor type ctor + Double tap.  
	```C#
	using Augusta_Tech___Rural_Sourcing.Data;
	using Microsoft.AspNetCore.Mvc;

	namespace Augusta_Tech___Rural_Sourcing.Controllers
	{
		[Route("api/[controller]")]
		public class BooksController: Controller
		{
			private IBookService _service;
			
			public BooksController(IBookService service)
			{
				_service = service;
			}
		}
	}
	```  

  ### a) Add - API Endpoint

  - Feature to add a new book 
  - Inside BooksController.cs add our first API endpoint. the ```AddBook``` method add a new API endpoint - send an HTTP post request; the URL for this request is AddBook - ```[HttpPost("AddBook")]```  
  - Pass as a body request book object that we want to add to our data - ```public IActionResult AddBook([FromBody]Book book)```  
  - Use the service that we just injected, to add our book to our collection - ```_service.AddBook(book);```  
  - Return a success response - ```return Ok("Added");```  
  BooksController.cs final code:
	```C#
	using Augusta_Tech___Rural_Sourcing.Data;
	using Microsoft.AspNetCore.Mvc;

	namespace Augusta_Tech___Rural_Sourcing.Controllers
	{
		[Route("api/[controller]")]
		public class BooksController: Controller
		{
			private IBookService _service;
			public BooksController(IBookService service)
			{
				_service = service;
			}

			//Create/Add a new book
			[HttpPost("AddBook")]
			public IActionResult AddBook([FromBody]Book book)
			{
				_service.AddBook(book);
				return Ok("Added");
			}
		}
	}
	```  
  - Return to ```BookService.cs``` and implemented the ```AddBook()``` method by adding the System.Collections.Generic.List```Add()``` method to add a book to our collection.in - (Adds the elements of the specified collection to the end of the)  
	```C#	
	public void AddBook(Book newBook)
	{
		Data.Books.Add(newBook);
	}
	```  

  ### b. Read All - API Endpoint

  - Feature to return all books. 
  - Inside BooksController.cs after the ```AddBook``` method add a new API endpoint - HTTP get request; the name of the URL is going to be the same as the action name - ```[HttpGet("[action]")]```  
  - Define the implementation; use the service and return them to the users  
	```C#
	//Read all books
	[HttpGet("[action]")]
	public IActionResult GetBooks(){
		var allBooks = _service.GetAllBooks();
		return Ok(allBooks);
	}
	```  
  - Return to ```BookService.cs``` and implemented the ```GetAllBooks()``` method; return the Books collection with the ToList() by importing System.Link namespace.  
	```C#
	public List<Book> GetAllBooks()
	{
		return Data.Books.ToList();
	}
	```  

  ### c. Update - API Endpoint

  - Feature to update a book.  
  - Inside BooksController.cs after the ```GetBooks``` method add a new API endpoint - HTTP put request; define the URL name with the endpoint UpdateBook and pass in the book ID as the parameter - ```[HttpPut("UpdateBook/{id}")]```  
  - Define the implementation; use the service and return the Okay book. 
	```C#
	//Update an existing book
	[HttpPut("UpdateBook/{id}")]
	public IActionResult UpdateBook(int id, [FromBody]Book book)
	{
		_service.UpdateBook(id, book);
		return Ok(book);
	}
	```
  - Return to ```BookService.cs``` and implemented the ```UpdateBook()``` method; So before we update a book, we need to first get the old data. For that use the Data.Books.Firstordefault and goes to n.id is equal to the ID parameter. Now we check if we have an existing book. So if the old book is different from null, we are going to update this book.
	```C#
	public void UpdateBook(int id, Book newBook)
	{
		var oldBook = Data.Books.FirstOrDefault(n => n.Id == id);
		if(oldBook != null)
		{
			oldBook.Title = newBook.Title;
			oldBook.Author = newBook.Author;
			oldBook.Description = newBook.Description;
			oldBook.Rate = newBook.Rate;
			oldBook.DateStart = newBook.DateStart;
			oldBook.DateRead = newBook.DateRead;
		}
	}
	```

  ### d. Delete - API Endpoint

  - Feature to delete a book. 
  - Inside BooksController.cs after the ```UpdateBook``` method add a new API endpoint - HTTP delete request; define the URL name with the API endpoint DeleteBook and pass in the book ID as the parameter - ```[HttpDelete("DeleteBook/{id}")]```  
  - Definte the implementation; use the service and return Ok as the result.
	```C#
	//Delete a book
	[HttpDelete("DeleteBook/{id}")]
	public IActionResult DeleteBook(int id)
	{
		_service.DeleteBook(id);
		return Ok();
	}
	```  
  - Return to ```BookService.cs``` and implemented the ```DeleteBook()``` method; find the book, and then remove this book from our collection.  
	```C#
	public void DeleteBook(int id)
	{
		var book = Data.Books.FirstOrDefault(n => n.Id == id);
		Data.Books.Remove(book);
	}
	```  
	
  ### e. Read Single - API Endpoint 

  - Feature to retreive a book. 
  - Inside BooksController.cs after the ```DeleteBook``` method add a new API endpoint - HTTP get request; define the URL name with the API endpoint SingleBook and pass in the book ID as the parameter - ```[HttpGet("SingleBook/{id}")]```  
  - Define the implementation; use the service and return Ok book.  
	```C#
	//Get a single book by id
	[HttpGet("SingleBook/{id}")]
	public IActionResult GetBookById(int id)
	{
		var book = _service.GetBookById(id);
		return Ok(book);
	}
	```  
  - Return to ```BookService.cs``` and implemented the ```GetBookById()``` method; return from the data that books where the book ID is first or default.  
	```C#
	public Book GetBookById(int id)
	{
		return Data.Books.FirstOrDefault(n => n.Id == id);
	}
	```  
  ### Testing API endpoints using Postman
  - run the application in VS Code terminal - ```dotnet run```  
  - copy the local host link - https://localhost:5001
  - open Postman and create a new Http Request:  
    - Get request to return all books - https://localhost:5001/api/Books/GetBooks
	- Get request to return a single book - https://localhost:5001/api/Books/SingleBook/1
	- Put request to update a book - https://localhost:5001/api/Books/UpdateBook/1  
	  Click the Headers tab and type in Content-Type as the Key, and application/json as the value;  
	  Click the Body tab and select Raw; copy and paste the book here you want to update; modify this book and Send  
	- Post request to add a new book - https://localhost:5001/api/Books/AddBook  
	  Click the Body tab and select Raw and type in a new book; Send and we should get an Added response.
	- Delete request to delete a book - https://localhost:5001/api/Books/DeleteBook/20  

<br/>  

## C. Getting Started with Angular

### 1. Angular Key Concepts  
<br/>  

**The main concepts in Angular are:**  

	#### Modules
	Blocks of functionalities that belong together.  
	An important Angular module which comes by default when generating an Angular app, is the *app module*, which is also known as the *config module*, as here we get to configure the components, providers, bootstrappers, etc.  
	```TypeScript
	import { BrowserModule } from '@angular/platform-browser';
	import { NgModule } from '@angular/core';
	@NgModule({
	  declarations: [ AppComponent ],
	  imports: [ BrowserModule ],
	  providers: [ Logger ],
	  bootstrap: [ AppComponent ]
	})
	export class AppModule { }
	```
	#### Components
	Defines the behavior of a portion of a screen.  
	Inside a component we have:
	- Template - a template is an HTML that defines how the view for that component is rendered.
	- Directive - are custom attributes that enhance the HTML syntax and are used to attach behaviors to specific elements on that page.
	- Data Binding - is a process that connects a component to its template and allows data and events to flow between them. 
	A component is decorated with the @ component syntax. Inside the component we define:  
	- the selector - which is a name that we want to use to render the view
	- the templateURL - which is the HTML file for this component  
	- providers for services
	- and also we can define in here the CSS code, which is going to be applied to this HTML only.  
	```TypeScript  
	@Component({
	  selector: 'app-home',
	  templateUrl: './home.component.html',
	})
	export class HomeComponent {
	}
	```  
	#### Services
	Define reusable functionalities that are independent of the views. This means that we can use a single service in different components.  
	*Dependency injection* is a way to supply dependencies and to supply services to different classes or components, we use dependency injection, and to use a service in Angular, we need to inject it in the components constructor.






		  
