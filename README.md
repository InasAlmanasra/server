# Server
Course Name: Web&Mobile                                                                                                               

Course Code: CEGEG077                                                                                                                   

Course Coordinator: Claire Ellul.                                                                                                          

Part three of Location Based Quiz Assignment (Server):This is the server repository; httpServer for location-based quiz.               


The code is also based on Dr.Claire Ellul practical sessions codes, particularly week 6 and week 7 practicals,URL: https://github.com/claireellul/cegeg077-week6formcode                                                                                     

Week 5 practicale, URL: https://github.com/claireellul/cegeg077-week5server



# Technical Guide

# Introduction

This is a
location-based quiz app that consists of two parts (Question-setting /quiz-app)
and the servers. 

Firstly;
the question setting app where the admin uses a web-based application to choose
the points that they desire. For each point; admin would click on its location
on the leaflet map to see the longitude and latitude of that point; then copy
its coordinates to the required fields. Admin also needs to set a question;
list four possible solutions and then tick which option is the correct one from
the four options he/she has chosen. 
This web-app will then upload the data and store it in a database table called
“quiz” and alert the user that their point has been successfully uploaded and
shows a “row inserted” message to confirm.

Secondly;
the quiz-app, a phone application which tracks the location of the user and
asks them a question if they were close enough to one. User would ask the app
to show him the location of the points by pressing the “show me points” and
walk towards those points. In case the user is far, an alert would show the
user that he needs to be at a closer distance. And if the user is indeed close
app would alert the user by telling the game is about to start and show the
question along with possible answers at the bottom of the app.
The user then proceeds to filling the quiz and press upload data, that would
upload the question, users answer and the right answer to a database table
called “useranswers”.

# Tools and requirements used to set and conduct the quiz-app;

# Hardware’s used:

Computer or
a laptop and an Android phone 5 or above for the second part of the
application.

# Software’s used:

Bitvise
SSH, Github

# Language’s used:

Javascript,
HTML and SQL

# Database used:

PostGres
SQL, the following two tables are generated: 

quiz table which contains the
following columns: question, opta, optb, optc, optd, answer and goem.Useranswer table which contains the
following columns: requiredquestion, answer and useranswer.



# Functions in the code:

Question
setting map

This map is
first imported and then created from leaflet API using CSS and javascript
codes. An additional div has been used to hold the map.

A marker is
added to locate where UCL is and the map is zoomed in to allow the user to
locate UCL easily, and a popup is used to show the user the coordinates of the
point they chose to click. 

Question-setting form

A
question-setting form is created below the leaflet map. First there are 5 text
boxes for the question and the 4 possible solutions. Below it there are the 4
radio buttons that the user need to check one of them to indicate which one of
the 4 options is the correct solution. Finally a longitude and latitude text
boxes are to be filled. Finally, the user hits start data upload button.

Uploading entered data to the database

As soon the
user hits the start data upload button; it calls for the startDataUpload
function in the uploadData.js that gets the values from html file and then
stores it in a postString variable that would be processed and sent a request
to upload this data.

In the
httpServer code an app.post for the uploadData function is created to insert
the required data into the database table.

Noting that
data is stored as 1.Integer in the case of the radio buttons. 2. String for the
question, options(1-4) and correct answer. 3. Geometry for the coordinates.

Location based quiz

This app
has several functionalities, including:

Track user’s location:

As the user
clicks the track your location button; the app locates the user by the
trackLocation function build on the navigator.geolocation that shows the real
time position of the user and highlights it through adding a small circle where
the user is on the map.

Show points of interest

The user
clicks on the show me points button; which connects to the database and shows
the location of the stored points on the map using getPOI function. 

The code
pulls a request to the getPOI function that accesses the information of the
database table as geoJSON data

In the
appActivity the code accesses all the features from the geoJSON and stores them
into a list and also returns variables that contain each feature separately (list
of questions, list of options (1-4), correct answer and goem).

Proximity to the stored Points

A for loop
is created to check if any of the points is close enough for the length of the
feature list and uses the calculateDistance function in the appActivity to
calculate the distance between the user coordinates and each point. Then the
closest point number is recorded in a new variable. This variable would be used
to access the respective question, possible answers and display them for the
user in the HTML file (if he is close enough).

Question-based game

The user
then start the game which would either tell him that he is too far and need to
get closer to the points, or simply alerts him that the game is about to start,
then the user sees the question retrieved and a radio button of possible
answers from the database, the user is only allowed to check one option and hit
upload data that would upload the data to another database table, same way as
mentioned above. 

An alert is
then displayed that his code is successfully uploaded and shows him the correct
answer, this alert is created in the uploadData server.

Server

For the
above two tasks to be done one must run the httpServer at all times. Hence,
typing “ node httpServer.js&” each time the apps need to be ran. 

What does
the httpServer do?

For the
first part (Question-Setting): it gets the information (questions, options
(1-4), correct answer and coordinates) from the index.html file, performs the appropriate
conversion task for coordinates to geom and inserts it to database table “quiz”.

For the
second part (proximity quiz-app): it gets the above information from “quiz”
database through getPOI app.get to the quiz app. 
Gets the user information (question close to the point, user answer and correct
answer) from the index.html file and inserts it to database table “useranswer”.

In both of
the above cases httpServer acts as a connection to postGIS database and a node
js programme that connects between client, server and database.  

The later
feature of httpServer is achieved through express, bodyParser functions.

