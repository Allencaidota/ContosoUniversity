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
