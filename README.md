<p float="left">
  <img src="https://github.com/user-attachments/assets/ae30d0bd-23c8-4c3a-ae70-297fa64aaa28" width="45%" />
  <img src="https://github.com/user-attachments/assets/4809bef3-c670-4f8e-86ca-45478338b365" width="45%" />
</p>

<p float="left">
  <img src="https://github.com/user-attachments/assets/fa10e9fd-d8cb-41a3-8135-20e210064e1f" width="45%" />
  <img src="https://github.com/user-attachments/assets/3c6e0dad-4ce4-47bd-b1a4-8fea27fa239c" width="45%" />
</p>

<p float="left">
  <img src="https://github.com/user-attachments/assets/97a7c608-a96b-4ca9-a6eb-bfccd1a7ea7f" width="45%" />
  <img src="https://github.com/user-attachments/assets/74b4e3fc-842a-44a3-b0f3-f4ff619af7fc" width="45%" />
</p>

<p float="left">
  <img width="203" alt="Screenshot 2024-08-23 at 12 07 24 PM" src="https://github.com/user-attachments/assets/92901806-20e5-4c11-9585-d8f3b0efc02a">
</p>



# StudyGroup App

## Overview
StudyGroup is a Django-based web application to connect users. Users can create and join discussion rooms, post messages, and participate in various topics.

## Features
-User Authentication: Users can register, log in, and log out securely.

-User Profiles: Users can update their profiles, including uploading an avatar and updating their bio.

-Discussion Rooms: Users can create, update, and delete discussion rooms. They can also join rooms to participate in discussions.

-Topics: Rooms can be categorized by topics, and users can filter rooms by topic.

-Messaging: Users can post messages in rooms, and messages are updated in real time.

-Mobile Friendly

-REST API: Provides endpoints for accessing rooms and topics programmatically.


## Project Structure

-views.py: Contains the main logic for handling user requests and rendering the appropriate HTML templates.

-models.py: Defines the database models such as User, Room, Topic, and Message.

-forms.py: Contains Django forms used for user registration and room creation.

-script.js: JavaScript file for handling client-side interactions like dropdown menus, image uploads, and auto-scrolling in chat rooms.



## Setup


-Clone the repository

-Install the required dependencies
```
pip install asgiref==3.8.1
pip install django==5.1
pip install djangorestframework==3.15.2
pip install django-cors-headers==4.4.0
pip install Pillow==10.4.0
pip install sqlparse
```

-Run migrations to set up the database

Start the development server:
```
python manage.py runserver
```



## Usage

-Register a new account or log in with an existing account.

-Create a new room or join an existing room to start discussing.

-Navigate through different topics to find rooms of interest.

-Use the REST API to interact with the application programmatically.


### base/views.py


1.```loginPage(request)```
Purpose: Handles user login.
Functionality:
Checks if the user is already authenticated. If so, redirects to the home page.
If not, processes the login form submission. It retrieves the email and password from the request, attempts to authenticate the user, and logs them in if successful.
If authentication fails, an error message is displayed.
Renders the login page template with a context indicating it's the login page.


2.```logoutUser(request)```
Purpose: Logs out the user.
Functionality:
Uses Django's logout function to log the user out and then redirects to the home page.


3.```registerPage(request)```
Purpose: Handles user registration.
Functionality:
Initializes a user creation form. If the request method is POST, it validates the form data.
If the form is valid, it saves the new user, logs them in, and redirects to the home page.
If not valid, displays an error message.
Renders the registration page template with the registration form.


4.```home(request)```
Purpose: Displays the homepage with a list of discussion rooms.
Functionality:
Retrieves a search query parameter (q) from the request to filter rooms by topic, name, or description.
Fetches topics and room messages related to the query.
Passes the rooms, room count, topics, and room messages to the home.html template for rendering.


5.```room(request, pk)```
Purpose: Displays a specific room and its messages.
Functionality:
Retrieves the room object using the primary key (pk).
Fetches all messages and participants related to the room.
Handles POST requests to add a new message to the room.
Renders the room.html template with the room details, messages, and participants.


6.```userProfile(request, pk)```
Purpose: Displays a user's profile.
Functionality:
Retrieves the user object using the primary key (pk).
Fetches all rooms and messages associated with the user.
Passes the user, rooms, messages, and topics to the profile.html template.


7.```updateUser(request)```
Purpose: Allows a user to update their profile information.
Functionality:
Initializes a form with the current user's information.
Processes form submissions to update the user's details.
Saves changes and redirects to the user's profile page if the form is valid.
Renders the edit-user.html template with the form.


8.```createRoom(request)```
Purpose: Allows users to create a new discussion room.
Functionality:
Initializes a room creation form.
On POST, retrieves or creates a topic, then creates a new room with the provided details.
Redirects to the home page after successfully creating a room.
Renders the room_form.html template with the form and available topics.


9.```updateRoom(request, pk)```
Purpose: Allows the host to update a room's details.
Functionality:
Retrieves the room using the primary key (pk) and initializes a form with its data.
Checks if the current user is the room's host.
Handles form submission to update the room's details, saves the changes, and redirects to the home page.
Renders the room_form.html template with the form, room, and topics.


10.```deleteRoom(request, pk)```
Purpose: Allows the host to delete a room.
Functionality:
Retrieves the room using the primary key (pk).
Checks if the current user is the room's host.
On POST, deletes the room and redirects to the home page.
Renders the delete.html template with the room details.


11.```deleteMessage(request, pk)```
Purpose: Allows users to delete their own messages.
Functionality:
Retrieves the message using the primary key (pk).
Checks if the current user is the message author.
On POST, deletes the message and redirects to the home page.
Renders the delete.html template with the message details.


12.```topicsPage(request)```
Purpose: Displays a list of topics.
Functionality:
Retrieves a search query parameter (q) to filter topics.
Fetches topics that match the query.
Renders the topics.html template with the topics.


13.```activityPage(request)```
Purpose: Displays recent activity in all rooms.
Functionality:
Retrieves all room messages.
Passes the messages to the activity.html template for rendering.





### api/views.py

1.```getRoutes(request)```
Purpose: Provides a list of available API routes.
Functionality:
Returns a static list of routes that describe the available API endpoints.


2.```getRooms(request)```
Purpose: Retrieves a list of all rooms.
Functionality:
Queries the database for all room instances.
Serializes the room data using the RoomSerializer.
Returns the serialized data as a JSON response.


3.```getRoom(request, pk)```
Purpose: Retrieves details of a specific room by its ID.
Functionality:
Fetches a room instance using the primary key (pk).
Serializes the room data using the RoomSerializer.
Returns the serialized data as a JSON response.




### studygroup/urls.py



1.```path('admin/', admin.site.urls)```
Maps the URL path admin/ to Django's built-in admin interface. This is standard for Django projects, providing access to the admin dashboard.


2.```path('', include('base.urls'))```
Maps the root URL (/) to the base app's URL configurations. This means any URL patterns defined in base.urls will be accessible directly from the root.

3.```path('api/', include('base.api.urls'))```
Maps the URL path starting with api/ to the URL configurations defined in base.api.urls. This means that the URLs for REST API endpoints will be accessible under the /api/ prefix.



### urls.py in the base App Directory



1.```path('', views.home, name='home')```
Maps the root URL to the home view. This serves as the homepage of the application where users can see a list of discussion rooms.


2.```path('login/', views.loginPage, name='login')```
Maps the login/ URL to the loginPage view. This URL allows users to log in.

3.```path('logout/', views.logoutUser, name='logout')```
Maps the logout/ URL to the logoutUser view. This URL logs users out and redirects them to the homepage.

4.```path('register/', views.registerPage, name='register')```
Maps the register/ URL to the registerPage view. This URL is used for user registration.

5.```path('room/<str:pk>/', views.room, name='room')```
Maps the room/<str:pk>/ URL to the room view, where <str:pk> represents the room's unique ID. This URL displays a specific room's details and messages.

6.```path('profile/<str:pk>/', views.userProfile, name='user-profile')```
Maps the profile/<str:pk>/ URL to the userProfile view, where <str:pk> represents the user's unique ID. This URL displays a user's profile, their rooms, and messages.

7.```path('create-room/', views.createRoom, name='create-room')```
Maps the create-room/ URL to the createRoom view. This URL allows users to create a new discussion room.

8.```path('update-room/<str:pk>/', views.updateRoom, name='update-room')```
Maps the update-room/<str:pk>/ URL to the updateRoom view, where <str:pk> represents the room's unique ID. This URL allows the host to update the room's details.

9.```path('delete-room/<str:pk>/', views.deleteRoom, name='delete-room')```
Maps the delete-room/<str:pk>/ URL to the deleteRoom view, where <str:pk> represents the room's unique ID. This URL allows the host to delete a room.

path('delete-message/<str:pk>/', views.deleteMessage, name='delete-message'):

Maps the delete-message/<str:pk>/ URL to the deleteMessage view, where <str:pk> represents the message's unique ID. This URL allows users to delete their own messages.
path('update-user/', views.updateUser, name='update-user'):

Maps the update-user/ URL to the updateUser view. This URL allows users to update their profile information.
path('topics/', views.topicsPage, name='topics'):

Maps the topics/ URL to the topicsPage view. This URL displays a list of all topics.
path('activity/', views.activityPage, name='activity'):

Maps the activity/ URL to the activityPage view. This URL shows recent activity and messages across all rooms.
urls.py in the api Subdirectory (within base)
This file typically defines URL patterns for API endpoints, which are part of the REST API functionality.

python
Copy code
from django.urls import path
from . import views

urlpatterns = [
    path('', views.getRoutes, name='routes'),
    path('rooms/', views.getRooms, name='rooms'),
    path('rooms/<str:pk>/', views.getRoom, name='room'),
]
Explanation:

path('', views.getRoutes, name='routes'):

Maps the root API URL (/api/) to the getRoutes view. This URL provides a list of available API routes for developers.
path('rooms/', views.getRooms, name='rooms'):

Maps the rooms/ URL to the getRooms view. This URL retrieves and displays a list of all rooms in JSON format.
path('rooms/<str:pk>/', views.getRoom, name='room'):

Maps the rooms/<str:pk>/ URL to the getRoom view, where <str:pk> represents the room's unique ID. This URL retrieves and displays details of a specific room in JSON format.











rest api stuff

<p float="left">
  <img src="https://github.com/user-attachments/assets/0180b74f-6da8-4d49-8578-7b7feafba7a0" width="45%" />
  <img src="https://github.com/user-attachments/assets/5c073482-00b8-4911-9563-2ac44c2e3f9b" width="45%" />
</p>
