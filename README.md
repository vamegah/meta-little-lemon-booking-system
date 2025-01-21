Connect the Little Lemon back-end to MySQL

Objectives
Create new MySQL database credentials

Update the project settings in Django to enable connection with MySQL 

Migrate models and update the database table

Introduction
In this lab, you will create and connect to an external MySQL database that can be used inside a Django project. You will migrate your models and populate data inside the database with the booking times available on the Little Lemon website. 

To do this, you will have to create a booking option on the Little Lemon website inside the Django project using the JavaScript functionalities in the template and the MySQL database as the back-end. This is part one of the final assessment where you will be making the database connections.  

Initial lab instructions
This lab will require you to modify the following files: 

settings.py 

directory structure in VSCode showing settings.py file.
As well as models.py.

Additionally, you are required to use the command line console inside the VS Code terminal. 

If not open already, go to Terminal on the Menu bar at the top of your screen and select New Terminal. 

New Terminal option inside the Terminal menu in VSCode
Before you begin this lab, you must activate the virtual environment. 

You have already built the project named myproject and added an app inside the project called myapp. 

Follow the instructions below and ensure you check the output at every step. 

Steps
Note: Make sure you have installed MySQL on your local machine and set up the admin user. In this lab, to keep it simple, you are going to begin with the root user with credentials. The root user by default has the password which is either password or <blank>. In case of blank password, simply press Enter. Also, note that the password will not be visible on screen as it’s being typed. 

Step 1: 

First go to the command line or terminal on your local machine and login to the mysql shell by typing the command:

mysql -u root -p

Press Enter and enter the password when prompted.

Note: Depending on the local machine, in some cases you may have to provide admin privileges for this command. This can be done by adding the keyword sudo before the command. For example:

sudo mysql -u root -p

Step 2:

Create a database with the name reservations with the command below:

CREATE DATABASE reservations;

Note: The commands in MySQL should end with a semi-colon (;).

 Step 3:

Next, you need to verify that the database has been created by typing the command:

SHOW DATABASES;

The output will depend on the databases present on your local machine. Confirm that it includes the database you have created.

Step 4:

Now open VS Code and open the Terminal inside it. Navigate to the project directory of your Django project and enable pipenv virtual environment for the project. 

Note: Ensure the Pipfile created by pipenv command is updated with mysqlclient and Django package installations along with the other dependencies specific to your local machine, for running the project.

Step 5: 

In the terminal, run the command that will enable access to mysql and enter the mysql shell by passing the -u and -p flags that will enable the MySQL console to prompt for a password. 

Prompt requesting password after running command mysql -u root -p
Note: The default password set for mysql here should be the same as the one set for root user on your local machine.

Step 6: 

Run the command to show databases and ensure the reservations database you have created is present in the list of databases.

Step 7: 

Create a new user for the database by running the following command:

 CREATE USER 'admindjango'@'localhost' IDENTIFIED BY 'employee@123!'; 

Step 8: 

Run the following command to grant all permissions to the user you have created:

 GRANT ALL ON *.* TO 'admindjango'@'localhost';

Result of running query to grant all permissions to a user in MySQL
Note: The full privileges are assigned here, but it is not the ideal practice in production environments.

Step 9: 

Run the command to flush all the privileges.

Note: Privileges assigned using GRANT command do not require the flush privileges but it is usually a good practice to run this command while you are using variable commands for changing privileges and reloading the server and grant tables containing information about user accounts. 

Step 10: 

Run the command to exit the mysql shell. 

Note: Once inside the Django shell, make sure the pipenv virtual environment is still active. You can observe the round brackets such as (myproject) as a suffix inside your VS Code prompt. 

Step 11: 

Inside your Django project, open the settings.py file and go to the INSTALLED_APPS configuration.

Add the name of your Django app myapp to the list. 

Configuration for INSTALLED APPS inside the settings.py file in VSCode.
Note: Make sure you add a comma(,) after the string. 

Step 12:

Now search for the configuration labeled DATABASES and update the configuration. Select the values assigned to it by default and replace them with the following code:

DATABASES configuration option under settings.py file
Step 13: After you enter the values and your code is the same as seen in the screenshot above, replace the values for the following:

NAME:

‘reservations’

USER:

'admindjango'

PASSWORD:

'employee@123!'

ENGINE

'django.db.backends.mysql'

Save the settings.py file.

Step 14: 

In the Terminal, run the command to make migrations. Make sure you are inside the directory that contains manage.py file.

Step 16:

Open the file models.py and take note of the code already added for creating a model named Booking. 

Step 17: 

Run both of the commands to perform the migration.

The migrations will create the Booking table that can be seen from the MySQL extension installed inside your VS Code.

Step 18:

Run the command to enter the mysql shell again and enter the credentials requested. 

Once inside the mysql shell, run a command to see the Booking table you have created. You can do this by typing the command:

use reservations;

Add a second command next:

show tables;

You can observe all the different tables that Django created after the migrate command. The main table that you will be dealing with from the list is the myapp_booking.

To see the details of this table, type the command:

describe myapp_booking;


Set up a Little Lemon booking API

Objectives
Create a view to process form data entered in a Django template

Convert form data received from a POST method into a JSON object and return to a web page

Introduction
Part one of the assessment included setting up a MySQL database which is far more secure and scalable than the SQLite which was used earlier. Now, in the part two of the final assessment, you have to integrate the booking feature into the existing website. 

Initial lab instructions
This lab will require you to modify the following files: 

views.py

forms.py

urls.py (app-level)

templates/bookings.html

templates/book.html

Starter code has already been added to the following files:

settings.py

models.py

urls.py (app-level)

urls.py (project-level)

views.py (partially complete)

Note: Once you set up the project, make sure you review the contents of all the files and follow the steps accordingly. The zip file added here may contain some additional stylesheets and formatting, so you must use the attached file as the starter code instead of building on your existing project from part one. 

You have already built the project named littlelemon and added an app inside the project called restaurant. 

Follow the instructions below and ensure you check the output at every step.

Note: Make sure you have installed MySQL on your local machine and set up the admin user. In this exercise, to keep it simple, you are going to begin with the root user with credentials. The root user by default has the password which is either password or <blank>. In case of a blank password, simply press Enter. The password will not be visible on screen as it’s being typed. 

This is part two of the final assessment and it deals with utilizing the Django framework without any JavaScript. You will first create a model form from the model you created earlier and then update the form with some data. You will then process the form data and send it to the front-end as a JSON object.

Steps
Step 1:

Run the following command to activate the virtual environment:

pipenv shell

Note: Make sure you run the command in the main working directory containing the manage.py file. 

Step 2:

To make sure you have the necessary dependencies in place, run the following commands inside pipenv:

pipenv install django

pipenv install mysqlclient

Note: If you are facing problems with using the virtual environment, you can try running the project without using pipenv.

Step 3:  

Create a file in the restaurant directory called forms.py and import the following:  

The Booking model from the file models.py.

The module forms from the package django

Step 4:  

Still inside the forms.py file, create a class called BookingForm and pass the forms.ModelForm as an argument to it. 

Step 5:  

Inside the class BookingForm, create another class called Meta and declare the following attributes inside it:  

Model with the value Booking  

Field with the value "__all__"

Step 6:

In the terminal run both commands to perform the migrations.

Note: Make sure you have created the correct MySQL user, assigned privileges, and configured the same database before you perform the migrations. 

Run the necessary commands to make sure the user is created and can access the database.

Step 7:

Open the file views.py and observe the code present inside it for the different views on the Little Lemon website. This was part of the functionality that was added using just the Django framework. 

Remove the comments for the book() view function.

Also remove the commenting for the import statement below:

# from .forms import BookingForm

Observe the book() function that contains the functionality to accept the values added inside the form rendered on the page book.html.

Note: The necessary imports are already added inside the views.py file. 

Step 8:

Create a view function called bookings() and pass the request object to it as an argument. Follow the pseudo code below to add the necessary code for processing the form data. 

Inside the bookings() view function, add the following pseudo code:

Create a variable called date and assign it the following value:

request.GET.get('date',datetime.today().date())

Create a variable called bookings and assign it the following value:

Call objects.all() on the Booking class object. 

Create a variable called booking_json and assign it the following value:

serialize() function called over serializers. Pass the following arguments to the serialize() function

‘json’

bookings

Return the render() function and pass the following arguments to it:

request

"bookings.html" as a string

dictionary containing following key-value pairs:

Key: “bookings”

Value: booking_json

Save the views.py file and ensure the code has no errors. 

Step 9:

Go to the app-level urls.py file and remove the comment for the URL configuration of the view functions below: 

bookings 

book

Step 10:

Open the file book.html file present inside the templates folder and update the code as per the instructions below.

Add a heading using the <h1> tag that says, Make a reservation.

 Step 11: 

Now step inside the <script> tags and add the following pseudo code to update the book.html file. 

Call the log() function on console and pass the argument “Hello” inside it

Call the function getElementById() on the document and pass the string "id_reservation_date" to it as an argument. 

Suffix the code by adding a filter type="date" using a dot operator. 

Step 12:

Now open the bookings.html file present inside the templates folder and update the code as per the instructions below.

Add a heading using the <h1> tag that says All Reservations

Step 13:

Now step inside the <script> tags and add the following pseudo code to update the bookings.html file. 

Create a constant called bookings and assign it the value of parse() function called over JSON. Pass the following argument inside the parse() function:

'{{ bookings|safe }}'

Call the log() function on console and pass bookings to it as an argument. 

Create a constant called pretty_json and assign it the value of:

stringify() function called over JSON and pass the following arguments to it:

bookings

null

2

Note: Observe the code added inside the file bookings.html and save the file. Make sure there are no errors present in your code. 

Step 14:

Open the file index.html and look for the comment:

<!-- Add code here for book  -->

Replace the comment with the following code:

<a href="{% url 'book' %}">Book your table now</a>

Step 15:

Open the file _header.html inside the templates or partials folder and look for the comments:

<!-- Add code here for the book template -->

<!-- Add code here for the reservations template -->

Replace the comment with the following code:

<li><a href="{% url 'book' %}">Book</a></li>

<li><a href="{% url 'bookings' %}">Reservations</a></li>

 Step 16:

Add a command to run the server and go to the localhost URL.

Step 17:

Go to the tab Book and add three entries inside the form.

Step 18:

Once the entries are updated, go to the Reservations tab that you see in the menu bar. You will be able to observe the entries are updated. 

Display the Little Lemon available booking times

Objective
Implement changes for updating the HTML form data using JavaScript 

 Introduction
In parts one and two of the final assessment, you connected a model and built a form to accept reservation details from an end-user on the Little Lemon website. In this exercise, you must use JavaScript to perform the following actions:

Create new bookings and refresh current bookings

Refresh bookings for a date when the date is changed

Dynamically process available time slots

Initial lab instructions
This lab will require you to modify the following files:

views.py

templates/bookings.html

Starter code has already been added to the following files:

settings.py

forms.py

models.py

urls.py (app-level)

urls.py (project-level)

Once you set up the project, make sure you oversee the contents of all the files and follow the steps accordingly. The zip file added here may contain some additional stylesheets and formatting so it is recommended to use it as the starter code instead of building on your existing project from part two. 

You have already built the project named littlelemon and added an app inside the project called restaurant. 

Follow the instructions below and ensure you check the output at every step.

Note: Make sure you have installed MySQL on your local machine and set up the admin user. In this lab, to keep it simple, you are going to begin with the root user with credentials. The root user by default has the password which is either password or <blank>. In case of a blank password, simply press Enter. The password will not be visible on screen as it’s being typed. 

This is part three of the final assessment and it deals with utilizing JavaScript functionalities. It will include changes in the view logic and the booking template created for processing the booking form data. 

Steps
Step 1:

Run the following command to activate the virtual environment:

pipenv shell

Note: Make sure you run the command in the main working directory containing the manage.py file. 

Step 2:

To make sure you have the necessary dependencies in place, run the following commands inside pipenv:

pipenv install django

pipenv install mysqlclient

 Step 3: 

Make sure you check the code inside all the supporting files you will need for the exercise. It includes a few changes from the endpoint of part two. 

Step 4:  

In the terminal run both commands to perform the migrations.

Note: Make sure you have created the correct MySQL user, assigned privileges, and configured the same database before you perform the migrations. 

Run the necessary commands to make sure the user is created and can access the database. In case you are using the root user, make sure you have the correct password for the root user added inside the settings.py file

Note: The migrations performed here are not necessary as the changes are already in place. It is however a good practice to perform migrations before you begin working on a new code block.  

Step 5:

Open the file views.py and create a view function called bookings() below the @csrf_exempt decorator. Pass a request object to it as an argument. 

Note: The necessary imports are already added inside the views.py file. URL configurations are already in place at the app-level urls.py file for the view function.

Step 6:  

Inside the view function bookings(), add the following pseudo code:

If value of request.method is equal to “POST”

Create a variable called data and assign it the value of json.load() with the request object passed as an argument.

Create a variable called exist and assign the following value to it:

Booking.objects.filter(reservation_date=data['reservation_date']).filter( )

Add the code below inside the filter() function above as an argument:

Create a variable reservation_slot and assign the value:

data['reservation_slot']).exists()

If the value of the exist variable is False: 

Create a variable called booking and assign the value of the Booking() class object with the following code passed inside it:

first_name=data['first_name'],

reservation_date=data['reservation_date'],

reservation_slot=data['reservation_slot'],

Call the save() function over the booking variable using the dot operator.

Else return HttpResponse() with the following arguments passed inside it:

"{'error':1}" as a string

Variable content_type with the value equal to 'application/json'

Create a variable called date and assign it the value: 

request.GET.get('date',datetime.today().date())

Create a variable called bookings and assign it the value:

Booking.objects.all().filter(reservation_date=date)

Create a variable called booking_json and assign it the value:

serializers.serialize('json', bookings)

Return HttpResponse() with the following arguments passed inside it:

booking_json

Variable content_type with the value equal to 'application/json'

Step 7:

Save the views.py file and ensure the code has no errors and that you have followed the pseudo-code accurately. 

Step 8:

Now step inside the book.html file and observe the code. There are three code blocks that you need to complete marked inside comments like:

<!-- Part 1 -->

 You will be replacing the comments with the pseudo-code as specified in following four steps below.

Step 9 (for part one):

Observe the code added inside the paragraph tag for the first name and replicate the code for the reservation date to replace the code block.  

Step 10 (for part two):

Call a function getElementById() over the document with ‘reservation_date’ passed inside it as an argument. 

Continue on the same block of code and call the function addEventListener() on it as a suffix using dot operator and pass the following arguments to it:

‘change’

function that contains the code: {getBookings()}

Step 11 (for part three):

Run a for loop on the constant data. Use a temporary variable called item to run the loop. Add the code inside the curly braces as follows:

Call a log() function over the console and pass item.fields as an argument

Call a push() function over reserved_slots array and pass the item.fields.reservation_slot to it as an argument

            Update the bookings string variable with the code below:

 `<p>${item.fields.first_name} - ${formatTime(item.fields.reservation_slot)}</p>`

Step 12 (for part four):

Create a variable called slot_options and assign the following string to it:

'<option value="0" disabled>Select time</option>'

Run a for loop for numbers greater than 10 and less than 20 and add the following code inside the curly braces:

Create a constant called label and assign the function formatTime() to it with i passed inside it as an argument.

If value of reserved.slots.includes(i) is true add the code:

slot_options += `<option value=${i} disabled>${label}</option>`

Else, add the code:

 slot_options += `<option value=${i}>${label}</option>`

Note: Make sure all the code blocks have appropriate curly braces where necessary.

Step 13:

Save the file and make sure there are no visible errors inside your HTML file. 

Now open the Terminal inside VS Code and add a command to run the server and go to the localhost URL and observe the web page. 

Step 14:

Enter your name in the text box and select a date and time of your choice to create a reservation. 

The screen should appear similar to the screen below:

 

Form for making reservations with name, reservation date and time fields and booking entries present
Step 15:

After selecting the options and pressing the Reserve Now button, you should be able to see the screen updated with your details. Note the times selected earlier are not available for selection.

Form displaying drop down option for selecting reservation time.
Step 16:

Note the change in the content displayed on the screen as per the change in the date selected:

Form with fields for reservation and form entries present for a different date.
Step 17:

Open the MySQL database and observe the entries updated in line with the changes in the reservations performed inside the template.