# ContosoUniversity
project:

7/27/2021
1.Changed the Layout From default to Contoso University which contains Home, About, Student, Courses, Instructors, and Departments pages.
2.Edited the index views to contoso page by using some bootstrap and html.
3.Install EF6 by using NuGet PM - Install-Package EntityFramework.



7/28/2021
1.create three enities : student, entrollment, and course
2.Create the database context which comunicates with my database 

7/29/2021
1. I Found out there is bug on student page which have error that the StudentController outlayer could't find
2. I solve this problem by re-create the studentcontroller and its views
3. This problem was cost by miss click that when i created the studentcontroller, i didnt leave the layerpage empty but fillin with studentcontroller which cost the error!


7/30/2021
1. try to finish the student crud pages
2. Detail page: nothing fancy, just add some table and data showup to the detail page with razor and html code
3. Create page: edited the studentController in create method that i added try, catch to post info if the create is not valid 
4. FunPart: it is very bad to pass the value by entity because even you didnt show your entity in the html hacker can still use tool such as fiddler to exuctue your property
            the we can solve this by using BindAttribute or use view models rather than entity classes with model binding
            
7/31/2021
try to work on the CRUD functionailty
1. edit page: First, we need to edit the edit function in the studentcontroller
              I overview the code from top find out db is schoolContext which is the database we set for the SchoolContext 
              the original one is not good enough for overposting, so i need to change the function
              I used ValidateAntiForgeryToken which  is an action filter that can be applied to an individual action, a controller, or globally. 
              Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.
2. What we did on the edit controller:
            checking if id is null, if yes we return to the httpstatus bad result, if !null we pass the updated vaule to db which is our schoolcontext class instance 
            Therefore, Using the tryUpdateModel which Updates the specified model instance using values from the controller's current value provider, a prefix, and included                 properties. Last we have db.savechange which casuse Entity Framework to create SQL statements to update the database row.
            At the end, we have to try catch to check if there is error
 
 
8/2/2021
try to finish the delete function 
1. First I edited the delete actionresult in the controller 
2. I optimize the delete page that if the delete id is null, the system will pop up a error message. To do that, i insert savechangesError boolean to check true or false. 
   Therefore, I need to add the viewBag error message to the delete views
3. I changed the delete action with adding a try catch. the httpPost Delete will try to find the student id in database, remove, and saveChange. if dataexcpetion pop error messege and reireact to index page.
4. Finally, we have the dispose function which Performs application-defined tasks associated with freeing, releasing, or resetting unmanaged resources.


8/3/2021 
Add sorting, filtering, and paging with the Entity Framework in an ASP.NET MVC application
1. add sorting to student index page:
             edited the student index in student controller
                        add sortOrder algorithm to controller by using    
                        ViewBag.NameSortParm = String.IsNullOrEmpty(sortOrder) ? "name_desc" : "";
                        ViewBag.DateSortParm = sortOrder == "Date" ? "date_desc" : "Date"; 
             next: using linq to select data from student db
             after getting the data we can sort the data in controller
2. edit the view index student page
            the table was displayname from model.name, fname, and enrollmentdate to actionlink
            
8/4/2021
Add a Search box
using the students from we created for the sortOrder funtion 
add a seachString parmater to the index controller method
check if string is null if not null we use "where" linq to search and use contains function to seach the string that pass by textbox
last we need to add textbox  to index view and search button

8/5/2021
add paging to the Students index page.
1. First: installing the PagedList.Mvc NuGet package - Install-Package PagedList.Mvc. 
The PagedList.Mvc package installs a paging helper that displays the paging buttons.
Second: Add paging functionality to the Index method
add a page parameter, a current sort order parameter, and a current filter parameter to the method
Because we may have muti pages in the student index and we need to deal with sort and search, we need to check if user has other action in different page
First, use currSort to save the sortOrder. next i need to check if the searchString is null or not because search will have impact on page dsiplay
lastly, we set the page size and pageNumber to the ToPageList method
2. since we change a lot in the controller we need to edit the view also
The @model statement at the top of the page specifies that the view now gets a PagedList object instead of a List object.
The using statement for PagedList.Mvc gives access to the MVC helper for the paging buttons.
The code uses an overload of BeginForm that allows it to specify FormMethod.Get.
For changing the page : @Html.PagedListPager( Model, page => Url.Action("Index", new { page }) )

8/6/2021
Create an about page

Create a view model class for the data that you need to pass to the view.
            EnrollmentDateGroup and student number in viewModel class for the view
Modify the About method in the Home controller.
            add namespace dal and viewModels to HomeController
            add student db context to homeController, and use it to get the studentd data by using LINQ statment groups to collect information
            return view(data.tolist());
            Add a Dispose method to release the garbage data;
Modify the About view.
            simplely just add a title name and table which can display the EnrollmentDate and studentCount.

8/11/2021
Enable connection resiliency
There is many problem could happen when your web in production such as load balancers. 
Those error is hard to aviod. as result, we can Use connection resiliency and command interception with Entity Framework in an ASP.NET MVC app.

1. create a SchoolConfiguration inheritence by DbConfiguration in DAL folder
2. There will be a constrator use SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy()); 
            it Creates a new instance of System.Data.Entity.SqlServer.SqlAzureExecutionStrategy.
3. add using System.Data.Entity.Infrastructure; to controller and change all the chatch to our "RetryLimitExceededException"
            such that :
                        catch (RetryLimitExceededException /* dex */)
{
    //Log the error (uncomment dex variable name and add a line here to write a log.
    ModelState.AddModelError("", "Unable to save changes. Try again, and if the problem persists see your system administrator.");
}




