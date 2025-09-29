
# Twender
    - [presentation](https://docs.google.com/presentation/d/1W8wUs0bUqKhZYc2ARwsxevkH3hzyMRJ93z3C47h44b4/edit?usp=sharing)

    - [source code](https://github.com/AlexNduta/Twender)
- A Django REST framework API designed to act as a virtual matatu(public transport system) conductor by managing payments and passenger trips.
- The application features role-based access for passengers and condutors

## Features
- User registration and JWT Authentication
- Role-Based access Control(passenger)
- Dynamic fare calculation based on dynamic routes, stops and peak-hours
- passenger endpoints to create trips
- conductor-only endpoint to view passenger destination and payment statuses
- conductor-only ability to update the payment status

## Structure
-Twender
    - Users App: Handles user registration, login an profiles

    - Trips Apps: Manages journeys - creating trips, calculating fares, view trip history

    - Payment App: Handlin payment logic
    
    - Conductors App: conductor app for viewing pasengers and confirming payments
![Twender-User-flow.png](Twender-User-flow.png)


# API documentation
- This API is documented using Swagger/OpenAPI. Once the project is running, the interactive documentation can be accessed at http://127.0.0.1:8000/api/schema/swagger-ui/

# Database Schema (ERD):
The database was designed to normalize route and user data. The Entity-Relationship Diagram can be viewed [here](https://drive.google.com/file/d/1TQM27c3QBKhCLC-oX_obyzpiyrLpI4F3/view?usp=drive_link)


# Getting Started / Local Development:

    - Prerequisites (`git`, `Python 3.10+`, `Docker`).

    - Clone the repository: git clone ...

    - Set up the `.env` file .

    - Build and run with Docker Compose: `docker-compose up --build`

    - Run database migrations: `docker-compose exec web python manage.py migrate`
# Technology Stack:

 - Backend: Django, Django REST Framework
 - Database: MySQL
 - Containerization: Docker, Docker Compose
 - API Documentation: drf-spectacular (Swagger/OpenAPI)
