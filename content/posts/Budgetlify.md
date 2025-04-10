+++
title = "Budgetlify"
date = "2024-01-25"
author = "Alex Kinyanjui"
+++

[Source code](https://github.com/AlexNduta/Budgetlify-API-SQL-)
This was a personal budgeting application to help me counter my poor budgeting and financial habits.

## Technologies
`Python`
`FastAPI`
`MySQL`
`Linux`

 {{< youtube oi7WxT6WvN8 >}}


- This is a budgeting app that enables you to create an account and track your regular spending, savings

Dir:
`API`: This is the backend section of the app
    - Queries the DB(currently using file storage) for data
    - Currently only accepts `Category`, `Date`, `Amount` and `Description`. The ID is auto generated
## Endpoints
    `Expense management`
| method | endpoint | purpose |
|--------| :-------:|--------:|
| POST | http://[hostname]/Budgetlify/api/v1.0/expense | create a new expense |
| GET | http://[hostanme]/Budgetlify/api/v1.0/expense | get a list of all expenses |
| GET | http://[hostname]/Budgetlify/api/v1.0/expenses{expense ID} | get a specific expense |
| PUT | http://[hostname]/Budgetlify/api/v1.0/expenses{expense ID} |  update an existing expense|
| DELETE | http://[hostname]/Budgetlify/api/v1.0/expenses{expense ID} | delete a specific expense |
|   `category management` |
| POST | http://[hostname]/Budgetlify/api/v1.0/categories | create categories |
| GET | http://[hostname]/Budgetlify/api/v1.0/categories | get a list of all categories
|      Monthly Report |
| GET | http://[hostname]/Budgetlify/api/v1.0/reports/{year}/{month} | Get reports for a specific month |
| `User Management` |
| GET | http://[hostname]/Budgetlify/api/v1.0/profile | Get info of an authenticated user |
| PUT | http://[hostname]/Budgetlify/api/v1.0/profile | update a user |
| `Login`|
| PUT | http://[hostname]/Budgetlify/api/v1.0/user | creates a new user |
| POST | http://[hostname]/Budgetlify/api/v1.0/Login | login a user |


`APP`: frontend android app for the user
        stack:
                - Android
                - Kotlin
                - JetpakCompose
