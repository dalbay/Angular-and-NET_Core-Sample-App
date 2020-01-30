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
	  
