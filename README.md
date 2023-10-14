# SQL Mastery through Instagram Data Modeling and Analysis Using PostgreSQL 
This project serves as a hands-on opportunity to advance your SQL expertise. As you model Instagram data in PostgreSQL, you'll strengthen your SQL skills by solving real challenges. Crafting intricate SQL queries to extract meaningful insights from user posts, comments, and likes will elevate your ability to handle relational databases effectively. You'll gain experience in aggregating data, implementing joins, and leveraging advanced SQL functions, all of which are vital for any SQL professional.

# What is Data Modeling?

Data modeling is the process of defining the structure of a database, including its tables, columns, relationships, and constraints. It helps ensure that data is organized, accurate, and efficient for storage and retrieval. Data modeling provides a blueprint for database developers, enabling them to create databases that meet the specific needs of various applications.

# Real-World Examples:

## Social Media App:
   In a social media app, data modeling manages diverse user interactions and content. Tables may consist of users (user ID, username, email), posts (post ID, user ID, content, timestamp), comments (comment ID, post ID, user ID, content, timestamp), likes (like ID, post ID, user ID, timestamp), and friendships (user ID, friend ID, status). Relationships link users to their posts, comments, likes, and friends. Constraints maintain data accuracy, like unique user IDs and valid post IDs. This structured model supports seamless user interactions, content sharing, and social networking functionalities.


## Hospital Management System:
   In a hospital management system, data modeling organizes extensive healthcare information. For instance, it defines tables for patients (containing patient ID, name, and contact details), doctors (doctor ID, specialization), appointments (patient ID, doctor ID, appointment time), medical records (patient ID, diagnosis, prescribed medications), and billing (patient ID, services availed, total cost). Relationships are established, linking patients to their doctors, appointments, and medical history. Constraints ensure data accuracy, such as unique patient IDs and appointment times. This structured model enhances patient care, appointment scheduling, and billing processes in healthcare facilities.

## Online Retail Store:
   In an online retail store, data modeling structures vast amounts of product and customer data. Tables can include products (product ID, name, price, description), customers (customer ID, name, shipping address), orders (order ID, customer ID, product ID, quantity, total cost), and payments (order ID, payment method, transaction status). Relationships connect customers to their orders and products. Constraints guarantee data integrity, such as unique product IDs and valid payment methods. This organized model facilitates inventory management, order processing, and personalized customer experiences.

# In This Tutorial, We Will Use A Real-World Example To Explain The Concept Of Data modeling and Build A SQL Script In PostgreSQL Using the Instagram Data Model Captured Below  :
Before we start building the tables, we need to create a data model to visualize the relationships between the data. In this case, we can create an entity-relationship (ER) diagram to represent the relationships between the users (user ID, username, email), posts (post ID, user ID, content, timestamp), comments (comment ID, post ID, user ID, content, timestamp), likes (like ID, post ID, user ID, timestamp), and followers (user ID, friend ID, status). The analysis of databases used by top Social networks showed that these SNSs used both, relational and NoSQL databases, although most use NoSQL. I am using a relational database model to help me in mastering SQL.

![Data-model-instagramdb](https://github.com/onuhmichael/SQL-Project-DB-Queries-Dashboard/assets/51151461/934a7966-7434-45d9-9dfa-c1df81d5563a)

## Choose a Database Management System (DBMS): We will be using PostgreSQL
PostgreSQL is a popular and powerful open-source PostgreSQL advanced, enterprise-class open-source relational database that supports both SQL (relational) and JSON (non-relational) querying.
Cost-Effective: Being open-source means PostgreSQL is free to use, making it a cost-effective choice for businesses and projects with budget constraints.
You can check out the link below for more information on how to set it up and use it.
## How to Set Up PostgreSQL and Create Databases (Step by Step)
https://www.youtube.com/watch?v=uoJjDbL-Y_E

##  Implement the Database Schema:
Create Tables/Collections: Implement the tables or collections in the database based on the entities in your data model.
Define Relationships: Establish relationships (foreign keys, references) between tables or collections.
Implement Constraints: Define constraints like unique, not null, default values, etc., to maintain data integrity.
# SQL Queries - Creating Tables

 CREATE TABLE Users (
    user_id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone_number VARCHAR(20) UNIQUE
);

 CREATE TABLE Posts (
    post_id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL,
    caption TEXT,
    image_url VARCHAR(200),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

 CREATE TABLE Comments (
    comment_id SERIAL PRIMARY KEY,
    post_id INTEGER NOT NULL,
    user_id INTEGER NOT NULL,
    comment_text TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (post_id) REFERENCES Posts(post_id),
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

 CREATE TABLE Likes (
    like_id SERIAL PRIMARY KEY,
    post_id INTEGER NOT NULL,
    user_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (post_id) REFERENCES Posts(post_id),
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

 CREATE TABLE Followers (
    follower_id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL,
    follower_user_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(user_id),
    FOREIGN KEY (follower_user_id) REFERENCES Users(user_id)
);
# SQL Queries - Populate the database with initial data based on the data model

## -- Inserting into Users table
INSERT INTO Users
(name, email, phone_number)
VALUES
    ('John Smith', 'johnsmith@gmail.com', '1234567890'),
    ('Jane Doe', 'janedoe@yahoo.com', '0987654321'),
    ('Bob Johnson', 'bjohnson@gmail.com', '1112223333'),
    ('Alice Brown', 'abrown@yahoo.com', NULL),
    ('Mike Davis', 'mdavis@gmail.com', '5556667777');

## -- Inserting into Posts table
INSERT INTO Posts (user_id, caption, image_url)
VALUES
    (1, 'Beautiful sunset', '<https://www.example.com/sunset.jpg>'),
    (2, 'My new puppy', '<https://www.example.com/puppy.jpg>'),
    (3, 'Delicious pizza', '<https://www.example.com/pizza.jpg>'),
    (4, 'Throwback to my vacation', '<https://www.example.com/vacation.jpg>'),
    (5, 'Amazing concert', '<https://www.example.com/concert.jpg>');

## -- Inserting into Comments table
INSERT INTO Comments (post_id, user_id, comment_text)
VALUES
    (1, 2, 'Wow! Stunning.'),
    (1, 3, 'Beautiful colors.'),
    (2, 1, 'What a cutie!'),
    (2, 4, 'Aww, I want one.'),
    (3, 5, 'Yum!'),
    (4, 1, 'Looks like an awesome trip.'),
    (5, 3, 'Wish I was there!');

## -- Inserting into Likes table
INSERT INTO Likes (post_id, user_id)
VALUES
    (1, 2),
    (1, 4),
    (2, 1),
    (2, 3),
    (3, 5),
    (4, 1),
    (4, 2),
    (4, 3),
    (5, 4),
    (5, 5);

## -- Inserting into Followers table
INSERT INTO Followers (user_id, follower_user_id)
VALUES
    (1, 2),
    (2, 1),
    (1, 3),
    (3, 1),
    (1, 4),
    (4, 1),
    (1, 5),
    (5, 1);

 # SQL Analytics Quary Examples from Instagram-db.: 
 ### Updating Data
### --(1.) Updating the caption of post_id 3 
UPDATE Posts 
SET caption = 'Awesome pizza ever'
 WHERE post_id = 3;


### Description:

This SQL command updates the "caption" column in the "Posts" table where the "post_id" is 3. The new caption value is set to 'Awesome pizza ever'. In the context of a database, this operation modifies an existing record.

### Explanation:

UPDATE Posts: Specifies that the operation is an update on the "Posts" table.
SET caption = 'Best pizza ever': Defines the column to be updated ("caption") and assigns a new value (' Awesome pizza ever') to it.
WHERE post_id = 3; Specifies a condition to identify the specific record(s) to be updated. In this case, it updates the record where the "post_id" is 3.
This SQL statement is often used when you want to modify specific data in a database table. In this example, it changes the caption of a post with ID 3 to 'Awesome pizza ever'. The WHERE clause is crucial as it ensures that only the intended record is updated, preventing unintended changes to other records in the table. This query helps maintain data accuracy and relevance within a database system.

### WHERE Condition
### (2)-- Selecting all the posts where user_id is 1
SELECT *
FROM Posts
WHERE user_id = 1;

### Description:

This SQL query retrieves data from the "Posts" table based on a specific condition using the WHERE clause. The condition specified is user_id = 1, which means it will select all the rows where the value in the "user_id" column is 1.

### Explanation:

SELECT * FROM Posts: This part of the query selects all columns (*) from the "Posts" table.
WHERE user_id = 1; This is the condition specified in the query. It filters the rows, ensuring only those where the value in the "user_id" column is 1 will be retrieved.
In this example, the query is tailored to fetch posts made by a specific user, identified by the user ID of 1. The WHERE clause acts as a filter, allowing the database to return only the rows that meet the specified condition. This is a fundamental SQL operation used for extracting specific and relevant data from large databases, enhancing the efficiency and accuracy of data retrieval processes.

### ORDER BY
### --(3) Selecting all the posts and ordering them by created_at in descending order
SELECT *
FROM Posts
ORDER BY created_at DESC;


### Description:

This SQL query demonstrates the usage of the ORDER BY clause to sort the results of a query in a specified order. In this example, the query selects all columns (*) from the "Posts" table and arranges them in descending order based on the "created_at" column.

### Explanation:

SELECT * FROM Posts: This part of the query selects all columns (*) from the "Posts" table.
ORDER BY created_at DESC;: This is the sorting condition specified in the query. It orders the selected rows based on the "created_at" column in descending (DESC) order.
In this scenario, the query is useful when you want to see the posts from the "Posts" table in reverse chronological order, with the newest posts appearing first. The ORDER BY clause is valuable for organizing query results, providing meaningful insights into data, and facilitating data analysis by presenting information in a structured manner.




    









