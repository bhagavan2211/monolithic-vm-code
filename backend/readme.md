Backend Application:-
---------------------
Backend Python Flask Application
This repository contains the backend code for a Python Flask application. The application serves as the server-side component, handling requests and providing data to the frontend of the web application. It is built using the Flask framework, which is a lightweight and flexible web framework for Python.

Getting Started
To get started with this backend application, follow the instructions below:

Services Involved:
------------------
cloud : AWS
services:
1.EC2
2.RDS

Prerequisites
---------------
We are running this application on top of VM. So we need to launch an instance.
Create a mysql database in rds
Install all the dependencies which are mentioned in dependencies.sh file in this repo

Procedure
---------------
------>first we have to create an ec2 instance for Backend application

------>Create the database in rds and allow 3306 from backend instance sg 
                    3306  --   backend-sg

Go through with the commands :

---->Connect to the instance

1)  yum install git -y 
 
2)  git clone <'git url link'>
 
3)  cd <'foldername'>
 
4)  cd <'backend flodername'>
 
5)  sh dependencies.sh  
 
---->After installing all the dependencies, connect with rds and create a table and then insert some values as your requirement,--for that follow the below commands
 
6)  mysql -h <'rds endpoint'> -u <'username'> -p <'password'>
 
7)  CREATE TABLE <'tablename'>( name VARCHAR(50) NOT NULL, roll INT NOT NULL, grade CHAR(1) NOT NULL );
 
8)  INSERT INTO <'tablename'>(name, roll, grade) VALUES ('leo hank', 103, 'A');
 
    INSERT INTO <'tablename'>(name, roll, grade) VALUES ('ram', 104, 'B');
 
    INSERT INTO <'tablename'>(name, roll, grade) VALUES ('devops', 105, 'B');
 
9)  \q ----to exit from the rds
 
 
 configuration:
--------------
 
    cofigure the application and give the rds creds i.e. host,username,password,db name
 
1)  vi properties.db-----
    update the dbname,host,username,password
 
2)  esc+:wq!-----command to save the file
    
Run the application:
----------------------
     a. foreground------
 
     1)  python3 app.py
  
     b. background------
  
     2)  nohup python3 app.py &
  
  
  