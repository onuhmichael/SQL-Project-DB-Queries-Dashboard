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

### GROUP BY and HAVING
### (4) -- Counting the number of likes for each post and showing only the posts with more than 2 likes.
SELECT Posts.post_id, COUNT(Likes.like_id) AS num_likes
FROM Posts
LEFT JOIN Likes ON Posts.post_id = Likes.post_id
GROUP BY Posts.post_id
HAVING COUNT(Likes.like_id) > 2;]

### Description:

This SQL query demonstrates the use of the GROUP BY and HAVING clauses to perform aggregations and filtering based on those aggregations. In this example, the query calculates the number of likes for each post and displays only the posts that have more than 2 likes.

### Explanation:

SELECT Posts.post_id, COUNT(Likes.like_id) AS num_likes: This part of the query selects the "post_id" from the "Posts" table and counts the number of corresponding "like_id" entries from the "Likes" table. The result is aliased as "num_likes" for clarity.
FROM Posts LEFT JOIN Likes ON Posts.post_id = Likes.post_id: This line combines data from the "Posts" table and the "Likes" table using a left join, ensuring that all posts are included in the result, even if they have no likes.
GROUP BY Posts.post_id: The GROUP BY clause groups the results by the "post_id" column. This means that the subsequent aggregate function operates on distinct posts.
HAVING COUNT(Likes.like_id) > 2; The HAVING clause filters the grouped results, displaying only the posts where the count of likes (num_likes) is greater than 2.
This query is useful when you want to identify popular posts based on the number of likes. The GROUP BY clause is essential for performing aggregations on specific columns, and the HAVING clause allows for filtering the aggregated results, providing valuable insights into the data.

### Aggregation Functions

### (5)-- Finding the total number of likes for all posts
SELECT SUM(num_likes) AS total_likes
FROM (
    SELECT COUNT(Likes.like_id) AS num_likes
    FROM Posts
    LEFT JOIN Likes ON Posts.post_id = Likes.post_id
    GROUP BY Posts.post_id
) AS likes_by_post;

### Description:

This SQL query demonstrates the use of aggregation functions, specifically the SUM() function, to calculate the total number of likes for all posts. It achieves this by first counting the number of likes for each post using the COUNT() function, then summing up these counts to get the overall total likes.

### Explanation:

SELECT COUNT(Likes.like_id) AS num_likes FROM Posts LEFT JOIN Likes ON Posts.post_id = Likes.post_id GROUP BY Posts.post_id: This inner query counts the number of likes for each post by joining the "Posts" and "Likes" tables on the "post_id" column, grouping the results by "post_id", and aliasing the count as "num_likes".
SELECT SUM(num_likes) AS total_likes FROM (...) AS likes_by_post;: The outer query calculates the sum of the "num_likes" column derived from the inner query. The inner query is treated as a subquery and aliased as "likes_by_post". The result, aliased as "total_likes", represents the total number of likes across all posts.
This query is valuable when you want to find the cumulative likes across all posts in a social media platform, blog, or any system where posts can be liked. It showcases the power of SQL's aggregation functions to perform complex calculations, providing a clear overview of the total engagement within the dataset.

### Subquery 
### (6)-- Finding all the users who have commented on post_id 1 

SELECT name FROM Users WHERE user_id IN ( SELECT user_id FROM Comments WHERE post_id = 1 );

### Description:

This SQL query demonstrates the use of a subquery to find all users who have commented on a specific post (post_id 1). It achieves this by first selecting user_ids from the "Comments" table where the post_id is 1 and then using these user_ids to fetch corresponding names from the "Users" table.

### Explanation:

SELECT user_id FROM Comments WHERE post_id = 1: This inner query selects all user_ids from the "Comments" table where the post_id is 1. This provides a list of user_ids who have commented on post_id 1.
SELECT name FROM Users WHERE user_id IN (...): The outer query uses the IN keyword to filter users whose user_id is present in the list obtained from the inner query. It selects the names of users who have commented on post_id 1.
This query is useful when you want to retrieve specific information about users who have interacted with a particular post, such as users who have commented on a specific article, image, or video. The subquery narrows down the user_ids related to the specified post, allowing for a targeted selection of user details from the "Users" table.

### JOINS
### (7) -- Which users have liked post_id 2?

SELECT Users.name
FROM Users
JOIN Likes ON Users.user_id = Likes.user_id
WHERE Likes.post_id = 2;

### -- Which posts have no comments?

SELECT Posts.caption
FROM Posts
LEFT JOIN Comments ON Posts.post_id = Comments.post_id
WHERE Comments.comment_id IS NULL;

### -- Which posts were created by users who have no followers?

SELECT Posts.caption
FROM Posts
JOIN Users ON Posts.user_id = Users.user_id
LEFT JOIN Followers ON Users.user_id = Followers.user_id
WHERE Followers.follower_id IS NULL;


### Description:

These SQL queries demonstrate the usage of different types of JOIN operations to extract specific information from related tables.

### Explanation:

Finding Users who Liked a Specific Post (post_id 2):

SELECT Users.name FROM Users JOIN Likes ON Users.user_id = Likes.user_id WHERE Likes.post_id = 2;
This query uses an INNER JOIN to combine the "Users" and "Likes" tables based on the common user_id. It selects the names of users who have liked the post with post_id = 2.
Finding Posts with No Comments:

SELECT Posts.caption FROM Posts LEFT JOIN Comments ON Posts.post_id = Comments.post_id WHERE Comments.comment_id IS NULL;
Here, a LEFT JOIN is utilized between the "Posts" and "Comments" tables on the post_id. The query selects post captions where there are no corresponding comments. The Comments.comment_id IS NULL condition ensures that only posts without comments are retrieved.
Finding Posts by Users with No Followers:

SELECT Posts. caption FROM Posts JOIN Users ON Posts.user_id = Users.user_id LEFT JOIN Followers ON Users.user_id = Followers.user_id WHERE Followers.follower_id IS NULL;
This query involves a JOIN between the "Posts" and "Users" tables using user_id. Additionally, it performs a LEFT JOIN with the "Followers" table to find posts created by users who have no followers. The condition Followers.follower_id IS NULL ensures that only posts by users without followers are selected.
These queries are valuable in scenarios where you need to extract specific information based on relationships between different entities. The JOIN operations help combine data from multiple tables, enabling complex analyses and detailed insights.

### Window Function 

### (8)-- Ranking the posts based on the number of likes
SELECT post_id, num_likes, RANK() OVER (ORDER BY num_likes DESC) AS rank
FROM (
    SELECT Posts.post_id, COUNT(Likes.like_id) AS num_likes
    FROM Posts
    LEFT JOIN Likes ON Posts.post_id = Likes.post_id
    GROUP BY Posts.post_id
) AS likes_by_post;

### Description:

In this SQL Analytics example, a window function is utilized to rank posts based on the number of likes they have received. The query calculates the total number of likes for each post by joining the Posts table with the Likes table on the post_id. It then groups the results by post_id, counting the number of likes for each post.

The outer query employs a window function, specifically the RANK() function, which assigns a rank to each post based on the descending order of the number of likes. The result set includes the post_id, the total number of likes (num_likes), and the rank assigned to each post.

### Explanation:

Inner Query:

Posts.post_id: Select the post ID from the Posts table.
COUNT(Likes.like_id) AS num_likes: Counts the number of likes for each post by joining the Posts table with the Likes table and counting the like IDs. This is done for each post separately due to the GROUP BY clause.
Outer Query:

RANK() OVER (ORDER BY num_likes DESC) AS rank: Applies the RANK() window function over the result set, ordering the posts by the number of likes in descending order. The RANK() function assigns a unique rank to each post based on its number of likes.
###  Example Usage:

Imagine you have a social media platform where users can like posts. Using this query, you can find out which posts have the most likes and rank them accordingly. The 'Rank' column will indicate the popularity of each post based on the number of likes it has received, allowing you to showcase the most liked posts to users on your platform.

### Case Statement 

### (9)-- Categorizing the posts based on the number of likes
SELECT
    post_id,
    CASE
        WHEN num_likes = 0 THEN 'No likes'
        WHEN num_likes < 5 THEN 'Few likes'
        WHEN num_likes < 10 THEN 'Some likes'
        ELSE 'Lots of likes'
    END AS like_category
FROM (
    SELECT Posts.post_id, COUNT(Likes.like_id) AS num_likes
    FROM Posts
    LEFT JOIN Likes ON Posts.post_id = Likes.post_id
    GROUP BY Posts.post_id
) AS likes_by_post;

### Description:

In this SQL Analytics example, a CASE statement is used to categorize posts based on the number of likes they have received. The query calculates the total number of likes for each post by joining the Posts table with the Likes table on the post_id. It then groups the results by post_id, counting the number of likes for each post.

The CASE statement is employed within the SELECT statement. It evaluates the value of 'num_likes' (the total likes for each post) and assigns a category to each post based on the following conditions:

If the post has 0 likes, it is categorized as 'No likes'.
If the post has less than 5 likes, it is categorized as 'Few likes'.
If the post has 5 or more but less than 10 likes, it is categorized as 'Some likes'.
If the post has 10 or more likes, it is categorized as 'Lots of likes'.
The resulting output includes the post_id and the corresponding like category assigned based on the number of likes for each post.

### Explanation:

This SQL query helps you analyze the popularity of posts on a social media platform based on the number of likes they have received. By categorizing posts into different groups such as 'No likes', 'Few likes', 'Some likes', or 'Lots of likes', you can quickly understand the engagement level of each post.

### Example Usage:

Imagine you have a blog platform where users can like posts. By using this query, you can categorize the posts into different groups, allowing you to identify posts that need more engagement and recognize highly popular posts easily. This categorization can be used to create tailored recommendations for users or to highlight trending content.

### Common Table Expression (CTE) 

### (10)-- Finding all the posts and their comments using a Common Table Expression (CTE)
WITH post_comments AS (
    SELECT Posts.post_id, Posts.caption, Comments.comment_text
    FROM Posts
    LEFT JOIN Comments ON Posts.post_id = Comments.post_id
)
SELECT *
FROM post_comments;

### Description:

In this SQL Analytics example, a Common Table Expression (CTE) is used to find all the posts and their corresponding comments. A CTE is a temporary result set that can be referenced within the context of a SELECT, INSERT, UPDATE, or DELETE statement. In this query, a CTE named 'post_comments' is created to join the Posts table with the Comments table on the post_id, retrieving the post_id, caption (post text), and comment_text for each post-comment pair.

After defining the CTE, the main SELECT statement selects all columns from the 'post_comments' CTE. This allows you to view a consolidated list of posts and their associated comments, providing a clear overview of the posts and the comments made on them.

### Explanation:

Using a CTE, this query simplifies the process of combining data from multiple tables. It first creates a virtual table ('post_comments') containing the post_id, post text, and comment text. This CTE acts as an intermediate step, making it easier to query and display the desired information. The final SELECT statement then retrieves all columns from this CTE, presenting a unified view of posts and comments.

### Example Usage:

Consider a scenario where you have a social media platform with posts and comments. By employing this query, you can quickly obtain a comprehensive list of posts along with the comments made on each post. This information can be valuable for content moderation, analysing user engagement, or enhancing user experience by showing posts and comments together.

### Date Casting and Working with Dates 

### (11) -- Finding all the posts created in the last month
SELECT *
FROM Posts
WHERE created_at >= CAST(DATE_TRUNC('month', CURRENT_TIMESTAMP - INTERVAL '1 month') AS DATE);

### Description:

In this SQL Analytics example, date casting and manipulation techniques are utilized to find posts created within the last month. The DATE_TRUNC function is employed to truncate the current timestamp to the beginning of the month, and then an interval of '1 month' is subtracted from it. This results in a date exactly one month ago. The CAST function is used to ensure that the truncated date and the created_at column in the Posts table are of the same data type for accurate comparison. The query then selects and displays all columns from the Posts table where the created_at date is greater than or equal to the calculated date from the last month.

### Explanation:

By using date functions and casting, this query effectively filters posts created within the last month. It starts by truncating the current timestamp to the beginning of the month and subtracting one month from it. This creates a specific date from one month ago. The CAST function is employed to convert.

### Conclusion:

Congratulations! You've completed a comprehensive journey into the world of SQL analytics. From mastering basic queries to delving into advanced data manipulation techniques, you've gained valuable insights into how SQL can be utilized to extract meaningful information from databases. Remember, SQL isn't just a tool; it's a language that empowers you to unravel intricate data puzzles.

Suggestions for Improvement:

Keep Practicing Regularly: Continue honing your SQL skills by practicing queries on various datasets. The more you practice, the more confident you become.

Explore Real-world Datasets: Dive into real-world datasets from platforms like Kaggle. This hands-on experience will not only enhance your problem-solving skills but also provide practical insights.

Engage with Coding Communities: Active participation in coding forums and communities offers new perspectives and solutions to challenges you might face.

Collaborate on Projects: Collaborating with others on SQL projects will not only enhance your teamwork skills but also expose you to diverse problem-solving approaches.

Challenge Yourself with Complex Queries: Tackle intricate scenarios to stretch your SQL skills and understanding. Your ability to solve complex problems will significantly improve.

Explore SQL Books and Tutorials: Explore SQL books and online tutorials tailored to your interests. Different resources offer unique insights that can deepen your understanding.

Teach Others: Consider explaining SQL concepts to friends or starting a blog. Teaching reinforces your understanding and helps solidify your knowledge.

Stay Updated: Stay abreast of the latest features and advancements in SQL and database technologies. Continuous learning ensures you are always equipped with the latest skills.

Seek Feedback: Don't hesitate to share your SQL solutions with experienced developers and seek feedback. Constructive criticism is invaluable for your growth.

Remember, your learning journey is ongoing. Embrace challenges, explore new horizons, and keep refining your SQL skills. Your proficiency in SQL will not only open doors to diverse career opportunities but also pave the way for exciting data-driven projects. Happy querying!















    









