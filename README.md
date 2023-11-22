----- ---------------------------Complete Developer Network (CDN)- Freelancers Directory-----------------------------------------------


------------------------------- Introduction ---------------------------------------------

CDN contains the list of freelancers so that they they could have a directory of contact get people for their job.
Contains details of the users such as phone number, skill sets, email Id etc.
Provision for creating, editing and deleting the user details.


--------------------------------Intallation setup, Technology and Project used -------------------------------------------

Installed Vs 2022, Microsoft SSMS 19.
Projects:
ASP.NET Core Web API -using C# and .NET 8.0 - For the server side -REST API - For building the CRUD Server(Create,read,update,delete).
ASP.NET Core Web App (Model-View-Controller) -(C#) - For building an MVC Client Application for the user interface.
Used Entity Framework
Database- SQL Server
pacakges- Microsoft.EntityFrameworkCore.SqlServer for SQL Server, Microsoft.EntityFrameworkCore.Tools

--------------------------------Usage----------------------------------------------------
The users tab contains the list of freelancers stored in the database.
Button-Create User- For registering the new user.
Create button -click- redirect to create user page- contains feilds such as Username, email, skillsets,hobby and phone number.
Each record contains edit,delete and details button.
Edit- on click- redirect to edit user page -For updating the user details when required
Details- on click-redirect to page containing the saved details of the user
Delete-on click- redirect to page asking for confirmation for delete and showing the details and the delete button
Each page containg the Back to List button also.

--------------------------------Code explaiantion--------------------------------------------------------------

-------------------------------Project 1- CdnFreelancersAPI( Template used-ASP.NET Core WebAPI)----------------------------------------------

1.Models folder-FUsers.cs

API Endpoints
Get All Users
Method: GET
Route: /api/users
Description: Retrieve a list of all users.
Request
No request parameters.

Response
Example response:
[
  {
    "id": 1,
    "username": "john_doe",
    "mail": "john@example.com",
    "phoneNumber": "+1234567890",
    "skillsets": "C#, ASP.NET Core",
    "hobby": "Coding"
  },
  // ... other users
]

2. Data folder-ApiDbContext.cs

The `ApiDbContext` class acts as a bridge between the CdnFreelancersAPI application and the underlying database. It provides a representation of the database schema, allowing for the creation, reading, updating, and deleting of records.
The ApiDbContext includes a DbSet<FUsers> property named Users. This property represents the collection of FUsers entities in the database. 

3.Program.cs
Code : builder.Services.AddDbContext<ApiDbContext>(options => options.UseSqlServer(builder.Configuration.GetConnectionString("DbConn")));

The AddDbContext method is used to register the ApiDbContext in the dependency injection container, specifying the use of SQL Server and providing the connection string named "DbConn".

4.appsettings.json
 Code:  "ConnectionStrings": {
    "DbConn": "Data Source=LAPTOP-EOROHOTU\\SQLEXPRESS; Initial Catalog=APICrudExample; User Id=lakshmi; Password=lakshmi; TrustServerCertificate=True;"
  },
Contains the connection string which specifies the server name and also the databse name.

5. Update the database
Take the package manager console- 
enter command - add migration "Createdb" (will create the migration folder containing the createdb migration in the solution)
enter command- update-database (for updtating the specified table into the database specified in the connection string)

6.Controller folder-UserController.cs

The `UserController` is responsible for handling HTTP requests related to user data. It interacts with the `ApiDbContext` to perform CRUD operations on the `FUsers` model.


Get User by ID
Method: GET

Route: /api/users/{id}

Description: Retrieve a user by their ID.

Request
Parameter: id - The ID of the user to retrieve.
Response
Example response:
{
  "id": 1,
  "username": "john_doe",
  "mail": "john@example.com",
  "phoneNumber": "+1234567890",
  "skillsets": "C#, ASP.NET Core",
  "hobby": "Coding"
}

Create User
Method: POST

Route: /api/users

Description: Create a new user.

Request
Body: User object with properties like username, mail, etc.
Response
Example response:
{
  "id": 2,
  "username": "jane_doe",
  "mail": "jane@example.com",
  "phoneNumber": "+9876543210",
  "skillsets": "Java, Python",
  "hobby": "Reading"
}

Update User
Method: PUT

Route: /api/users/{id}

Description: Update an existing user.

Request
Parameter: id - The ID of the user to update.
Body: Updated user object.
Response
No content. Returns 200 OK if successful.

Delete User
Method: DELETE

Route: /api/users/{id}

Description: Delete a user by their ID.

Request
Parameter: id - The ID of the user to delete.
Response
No content. Returns 200 OK if successful.

While running this project we will get all the specified APIs in the Swagger framework. We can test all the operations in this framework.


---------------------------------Project 2- CdnFreelancersClient (Template used- ASP.NET Core Web App (Model-View-Controller))---------------------------

1. Model folder-FUsers.cs
The `FUsers` model is a C# class which utilizes the `System.ComponentModel.DataAnnotations` namespace to apply data annotations for validation and database schema customization.It can be utilized in controllers, views, and services to handle user-related functionality.
Contains properties such as username, skillsets,phonenumber etc.

2. APIGateway.cs
 The `APIGateway` class in the project serves as a communication gateway between the ASP.NET Core Web App (MVC) application and the backend API, facilitating CRUD operations on user data. It encapsulates the logic for making HTTP requests to the backend API endpoints related to user data. It handles communication with the API, including error handling and data serialization.

Methods
ListUsers
Description: Retrieve a list of all users.
Usage: Invoked to fetch the list of users from the API.
Explanation : Initially it checks if the URL uses HTTPS and set the appropriate security protocol.Then sends an HTTP GET request to the specified URL. It reads the response content as a string and deserialize the JSON string into a list of FUsers objects. Finally it check if the deserialization was successful and update the users list

CreateUser
Description: Create a new user.
Usage: Invoked to send a request to create a new user to the API.

GetUser
Description: Retrieve details of a specific user.
Usage: Invoked to fetch user details from the API based on the user ID.

UpdateUser
Description: Update an existing user.
Usage: Invoked to send a request to update an existing user to the API.

DeleteUser
Description: Delete a user.
Usage: Invoked to send a request to delete a user to the API.


3. Controller folder- UserController.cs(MVC controller)
Will do the operations in the client side by connecting to the API Server. It interacts with the `APIGateway` service to perform CRUD operations on user data. It is responsible for managing views related to user actions.
The `UserController` is initialized with an instance of the `APIGateway` service through dependency injection. This service is responsible for communication with the backend API.


Actions
Index
Description: Display a list of all users.
Usage: Accessed via the /User/Index route.
Example:
public IActionResult Index()
{
    List<FUsers> users;
    users = apiGateway.ListUsers();
    return View(users);   
}

Create
Description: Display a form to create a new user.
Usage: Accessed via the /User/Create route.
Example:
[HttpGet]
public IActionResult Create()
{
    FUsers user = new FUsers();
    return View(user);
}

Details
Description: Display details of a specific user.
Usage: Accessed via the /User/Details/{id} route.
Example:
public IActionResult Details(int id)
{
    FUsers user = apiGateway.GetUser(id);
    return View(user);
}

Edit
Description: Display a form to edit an existing user.
Usage: Accessed via the /User/Edit/{id} route.
Example:
[HttpGet]
public IActionResult Edit(int id)
{
    FUsers user = apiGateway.GetUser(id);
    return View(user);
}
Delete
Description: Display a confirmation page to delete a user.
Usage: Accessed via the /User/Delete/{id} route.
Example:
[HttpGet]
public IActionResult Delete(int id)
{
    FUsers user = apiGateway.GetUser(id);
    return View(user);
}

4. Views> User folder >
 Index.cshtml,Create.cshtml, Details.cshtml,Edit.cshtml, Delete.cshtml 
  ASP.NET Core Razor for displaying a list of freelancers, creating new user, viewing user details,editing existing user, deleting existing user providing appropriate Bootstrap classes for better styling and responsiveness.
