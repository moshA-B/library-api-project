# library api

## this system handles a library by storing information about books and members it stores the information using sql databases and allows modification using fastapi endpoints


## to initiate the mysql database run the following:
docker run --name library-mysql -e MY_SQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=library_db -p 3306:3306 -d mysql:latest

## run to confirm: 
docker ps

#
### the folder format is:
```
library-api/  
│  
├── app/  
│   ├── main.py  
│   ├── database/  
│   │   ├── db_connection.py  
│   │   ├── book_db.py  
│   │   └── member_db.py  
│   ├── routes/  
│   │   ├── book_routes.py  
│   │   ├── member_routes.py  
│   │   └── report_routes.py  
│   └── logs/  
│       └── app.log  
│  
├── README.md  
├── requirements.txt  
└── .gitignore
```


# table format:

## books
|method| specifications|
|--- |----|
| id | int auto increment |
| title | book title, required, max 50 chars |
| author | author name, required, max 50 chars |
| genre | ENUM, can be Fiction/Non-Fiction/ Science/ History, required |
| is_available | True/False |
| borrowed_by_member | the member who borrowed default null |


## members
|method| specifications|
|--- |----|
| id | int auto increment |
| name | member name, required, max 50 chars |
| email | unique ,required |
| is_active | True/False |
| total_borrows | counts all borrows, increment on every borrow, required |



# system rules:
|rule no.|method|details|
|--- |----|---|
| 1 | book creation | user sends title/author/genre system adds is_available=True, borrowed_by=null |
| 2 | genre | must be Fiction / Non-Fiction / Science / History anything else returns error |
| 3 | member creation | user sends name/email system adds is_active=True, total_borrows=0 |
| 4 | email | must be unique if exists return error |
| 5 | non active member | can't borrow books |
| 6 | non available book | can't borrow if is_available=False |
| 7 | max books | member cant hold more then 3 books | 
| 8 | return books | only member who borrowed can return |


## endpoint list

### books

|method|url|action|
|----|----|----|
| POST | /books | book creation |
| GET | /books |  all books |
| GET | /books/{id} | book by id|
| PATCH | /books/{id} | update book by id|
| PATCH | /books/ {id}/borrow/{member_id} | barrow book |
| PATCH | /books/{id}/return/{member_id} | return book |

### members:
|method|url|action|
|---|---|---|
| POST | /members | create member |
| GET | /members | all members |
| GET | /members/{id} | member by id |
| PATCH | /members/{id} | update member |
| PATCH | /members/{id}/deactivate | 
| PATCH | /members/{id}/activate |

### reports
|method|url|action|
|---|---| ---|
| GET | /reports/summary | general report |
| GET | /reports/books-by-genre | books by genre | 
| GET | /reports/top-member | most active members | 



## program flow:

HTTP request -> fastapi -> endpoints -> query -> database

## run instructions

create a venv by python -m venv venv
###
run the venv
###
install the requirements by running pip install -r requirements.txt
