# Angular and ASP.NET Core Sample App

Tutorial on how to build an Angular - ASP.NET Core web application

**Application Overview:**

- build service layer with the ASP.NET Web API for the back end, to expose the required endpoints to create, read, update, and delete entries
- build a UI using Angular with Bootstrap;
- implement NgRx—a framework for building reactive applications in Angular—in your app;
- implement user authentication with Auth0.

## 1. .NET Core Templates

- Type in the terminal `$ dotnet new angular` to build a new angular project
- Run the project - `dotnet run` and click on the localhost link; this will open the build in template.

## 2. Web API architectural overview

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
- *Startup.cs* file which is also known as the configuration file; like the database connection strings, the services that we want to use,... Inside the Startup.cs we have two methods, the ```ConfigureServices``` and the ```Configure``` methods. The ConfigureSevices method is used to configure dependency interaction. And on the other hand we have the Configure method which is used to setup middle wares, routing rules,... So for example if we want to use a service in the future, we can configure it inside the ConfigureServices method.  

## 3. Angular Architectural Overview

- The default Angular files that were created inside the **ClientApp folder**:  
  1. **e2e** -  used for unit testing related code
  2. **node_modules** - libraries that we need to use to run our Angular app
  3. **src** - all the code goes in here; inside the source folder:  
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

## 4. Creating Data Models  

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



## 5. Adding a Service and Data
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

## Create an API Endpoint
- create a controller which will have the API Endpoints that we are going to need. 
- go to the Controllers folder and create a new controller- *BooksController.cs*
  - define the namespace and then inside here we are going to create our first controller
  - for a class to be a controller, we need to inherit in this case from the controller base class, which is the AspNetCore.mvc.
    First of all, let us define the route for this controller - ```[Route("api/[controller]")]```   
	Inside this controller, we want to use the books service. So, to use the service, we need to inject it in our constructor. For that, we write in here private IBookService. And then we write in here, ctor. Double tap to create the constructor. 
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
	- Now it's time to create our first API Endpoint, for creating or adding a new book. To add a new book, we are going to send an HTTP post request; the URL for this request is AddBook - ```[HttpPost("AddBook")]```  
	We want to pass as a body request; the book that we want to add - ```public IActionResult AddBook([FromBody]Book book)```  
	inside here we are going to use this service that we just injected, to add our book to our collection - ```_service.AddBook(book);```  
	and return a success response - ```return Ok("Added");```BooksController.cs final code:
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
	- implemented the ```Add()``` method in ```BookService.cs```- 
	```C#	
        public void AddBook(Book newBook)
        {
            Data.Books.Add(newBook);
        }
	```



	  
