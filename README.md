# Pizzarunner
Week 2 of 8 Week SQL challenge. This is part of the 8 week SQL Challenge, by Danny Ma. You can find his challenge here:
https://8weeksqlchallenge.com/case-study-2/


![pizzarunner](https://github.com/ThaisCooke/pizzarunner/blob/0fa6e6a215ed377ed6ca9600400d30a80c0b690e/pizza.png)


-- As I did in the first week of the challenge, I created the tables on Microsoft SQL Server


    CREATE TABLE customer_orders (
      "order_id" INTEGER,
      "customer_id" INTEGER,
      "pizza_id" INTEGER,
      "exclusions" VARCHAR(4),
      "extras" VARCHAR(4),
      "order_time" DATETIME DEFAULT CURRENT_TIMESTAMP
    );


      INSERT INTO customer_orders
      ("order_id", "customer_id", "pizza_id", "exclusions", "extras", "order_time")
    VALUES
    ('1', '101', '1', '', '', '2020-01-01 18:05:02'),
    ('2', '101', '1', '', '', '2020-01-01 19:00:52'),
    ('3', '102', '1', '', '', '2020-01-02 23:51:23'),
    ('3', '102', '2', '', NULL, '2020-01-02 23:51:23'),
    ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
    ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
    ('4', '103', '2', '4', '', '2020-01-04 13:23:46'),
    ('5', '104', '1', 'null', '1', '2020-01-08 21:00:29'),
    ('6', '101', '2', 'null', 'null', '2020-01-08 21:03:13'),
    ('7', '105', '2', 'null', '1', '2020-01-08 21:20:29'),
    ('8', '102', '1', 'null', 'null', '2020-01-09 23:54:33'),
    ('9', '103', '1', '4', '1, 5', '2020-01-10 11:22:59'),
    ('10', '104', '1', 'null', 'null', '2020-01-11 18:34:49'),
    ('10', '104', '1', '2, 6', '1, 4', '2020-01-11 18:34:49');    

    CREATE TABLE runners (
    "runner_id" INTEGER,
    "registration_date" DATE
    );
    INSERT INTO runners
    ("runner_id", "registration_date")
    VALUES
      (1, '2021-01-01'),
      (2, '2021-01-03'),
      (3, '2021-01-08'),
      (4, '2021-01-15');

    CREATE TABLE runner_orders (
      "order_id" INTEGER,
      "runner_id" INTEGER,
      "pickup_time" VARCHAR(19),
      "distance" VARCHAR(7),
      "duration" VARCHAR(10),
      "cancellation" VARCHAR(23)
      );

      INSERT INTO runner_orders
      ("order_id", "runner_id", "pickup_time", "distance", "duration", "cancellation")
      VALUES
      ('1', '1', '2020-01-01 18:15:34', '20km', '32 minutes', ''),
      ('2', '1', '2020-01-01 19:10:54', '20km', '27 minutes', ''),
      ('3', '1', '2020-01-03 00:12:37', '13.4km', '20 mins', NULL),
      ('4', '2', '2020-01-04 13:53:03', '23.4', '40', NULL),
      ('5', '3', '2020-01-08 21:10:57', '10', '15', NULL),
      ('6', '3', 'null', 'null', 'null', 'Restaurant Cancellation'),
      ('7', '2', '2020-01-08 21:30:45', '25km', '25mins', 'null'),
      ('8', '2', '2020-01-10 00:15:02', '23.4 km', '15 minute', 'null'),
      ('9', '2', 'null', 'null', 'null', 'Customer Cancellation'),
      ('10', '1', '2020-01-11 18:50:20', '10km', '10minutes', 'null');


      CREATE TABLE pizza_names (
      "pizza_id" INTEGER,
      "pizza_name" TEXT
      );

    INSERT INTO pizza_names
      ("pizza_id", "pizza_name")
    VALUES
      (1, 'Meatlovers'),
      (2, 'Vegetarian');

      CREATE TABLE pizza_recipes (
      "pizza_id" INTEGER,
      "toppings" TEXT
    );
      INSERT INTO pizza_recipes
      ("pizza_id", "toppings")
      VALUES
      (1, '1, 2, 3, 4, 5, 6, 8, 10'),
      (2, '4, 6, 7, 9, 11, 12');

      CREATE TABLE pizza_toppings (
      "topping_id" INTEGER,
      "topping_name" TEXT
    );
    INSERT INTO pizza_toppings
      ("topping_id", "topping_name")
      VALUES
      (1, 'Bacon'),
      (2, 'BBQ Sauce'),
      (3, 'Beef'),
      (4, 'Cheese'),
      (5, 'Chicken'),
      (6, 'Mushrooms'),
      (7, 'Onions'),
     (8, 'Pepperoni'),
      (9, 'Peppers'),
      (10, 'Salami'),
      (11, 'Tomatoes'),
      (12, 'Tomato Sauce');
      
  
  
  ## When inspecting the tables, there were a lot of "Null" values. I started by cleaning the tables before answering the questions.
  
  -- Cleaning "NULL" and "Null" values from runner_orders table:
  
        UPDATE runner_orders
        SET cancellation = ' '
        WHERE cancellation IS NULL

        UPDATE runner_orders
        SET cancellation = ' '
        WHERE cancellation = 'null'

        UPDATE runner_orders
        SET duration = ' '
        WHERE duration = 'null'

        UPDATE runner_orders
        SET distance = ' '
        WHERE distance = 'null'

        UPDATE runner_orders
        SET pickup_time = ' '
        WHERE pickup_time = 'null'
        
 
 -- Cleaning "NULL" and "Null" values from customer_orders table:
 
        UPDATE customer_orders
        SET exclusions = ' '
        WHERE exclusions = 'null'

        UPDATE customer_orders
        SET extras = ' '
        WHERE extras = 'null'

        UPDATE customer_orders
        SET extras = ' '
        WHERE extras IS NULL
  
  
  ### A. Pizza Metrics
  
  **Question 1**: How many pizzas were ordered?
  
	number_of_orders 
	       14
	      
                   
  
  -- Using the COUNT function:
  
        SELECT COUNT (*) AS number_of_orders
        FROM customer_orders
        
 
 **Question 2**: How many unique customer orders were made?
 
        customer_id	        unique_orders
               101	            3
               102	            2
               103	            2
               104	            2
               105	            1
             
             
 -- I used the functions COUNT DISTINCT and GROUP BY:
 
        SELECT customer_id, COUNT (Distinct order_id) AS unique_orders
        FROM customer_orders
        GROUP BY customer_id
        
        
**Question 3**: How many successful orders were delivered by each runner?

        runner_id	successful_orders
            1	            4
            2	            3
            3	            1
            
            
 -- I used the functions COUNT, WHERE and GROUP BY :
 
        SELECT runner_id, COUNT (order_id) AS successful_orders
        FROM runner_orders
        WHERE cancellation = ' '
        GROUP BY runner_id
        

**Question 4**: How many of each type of pizza was delivered?

        Type_of_pizza	    Number_of_orders
           Meatlovers	        10
           Vegetarian	        4


-- First, I joined the tables pizza_names and customer_orders:

        SELECT pizza_name, order_id, customer_id
        FROM pizza_names
        INNER JOIN customer_orders
        ON pizza_names.pizza_id = customer_orders.pizza_id
        

-- Then I used COUNT function, however, I was getting an error message:

*Msg 306, Level 16, State 2, Line 16 The text, ntext, and image data types cannot be compared or sorted, except when using IS NULL or LIKE operator*


-- It turned out that the pizza_names column was in **text**, while the other columns from the table customer_orders were in **varchar**. I wrote the same query, using the CAST function:

        SELECT CAST(pizza_name AS VARCHAR(100)) AS Type_of_pizza, COUNT (CAST (pizza_name AS VARCHAR(100))) Number_of_orders
        FROM pizza_names
        INNER JOIN customer_orders
        ON pizza_names.pizza_id = customer_orders.pizza_id
        GROUP BY CAST (pizza_name AS VARCHAR(100))
        
        
 -- That query gave me the desired result
 
 
 **Question 5**: How many Vegetarian and Meatlovers were ordered by each customer?
 
        customer_id	        Number_of_pizzas	        Type_of_pizza
            101	                    2	                    Meatlovers
            101	                    1	                    Vegetarian
            102	                    2	                    Meatlovers
            102	                    1	                    Vegetarian
            103	                    3	                    Meatlovers
            103	                    1	                    Vegetarian
            104	                    3	                    Meatlovers
            105	                    1	                    Vegetarian
 
 -- First, I joined the tables pizza_names and customer_orders:
 
        SELECT pizza_name, order_id, customer_id
        FROM pizza_names
        INNER JOIN customer_orders
        ON pizza_names.pizza_id = customer_orders.pizza_id
        
-- Then, I used the functions COUNT, GROUP BY and ORDER BY (remembering to use CAST for pizza_name column)


           SELECT customer_id, COUNT (*) AS Number_of_pizzas, CAST (pizza_name AS VARCHAR(100)) AS Type_of_pizza
		   FROM pizza_names
           INNER JOIN customer_orders
           ON pizza_names.pizza_id = customer_orders.pizza_id
		   GROUP BY customer_id, CAST (pizza_name AS VARCHAR(100))
		   ORDER BY customer_id



**Question 6**: What was the maximum number of pizzas delivered in a single order?

		number_of_pizzas
		        3


-- First, I joined the tables customer_orders and runner_orders, eliminating the orders that had been cancelled:

		SELECT dbo.customer_orders.order_id, distance, customer_id
  		FROM customer_orders
  		INNER JOIN runner_orders
  		ON customer_orders.order_id = runner_orders.order_id
  		WHERE distance <> ' '
		

-- Then, I did one table for counting the number of customers that had placed orders, using the COUNT function:

		SELECT dbo.runner_orders.order_id, COUNT (customer_id) AS pizzas_per_order
  		FROM customer_orders
  		INNER JOIN runner_orders
  		ON customer_orders.order_id = runner_orders.order_id
  		WHERE distance <> ' '
  		GROUP BY dbo.runner_orders.order_id
		
		
-- Next, I put that on a cte in order to use the functions COUNT and MAX together:

		WITH cte_pizza_count AS
  		(SELECT dbo.customer_orders.order_id, COUNT (customer_id) AS pizzas_per_order
  		FROM customer_orders
  		INNER JOIN runner_orders
  		ON customer_orders.order_id = runner_orders.order_id
  		WHERE distance <> ' '
  		GROUP BY dbo.customer_orders.order_id)

  		SELECT MAX (pizzas_per_order) AS number_of_pizzas
  		FROM cte_pizza_count


**Question 7**: For each customer, how many delivered pizzas had at least 1 change and how many had no changes?


		customer_id	at_least_1_change	no_change
		    101	               0	           2
		    102	               0	           3
		    103	               3	           0
		    104	               2	           1
		    105	               1	           0

-- First, I joined the tables runner_orders and customer_orders, in order to select only the pizzas that had been delivered:

		SELECT dbo.runner_orders.order_id, customer_id, exclusions, extras
  		FROM runner_orders
  		INNER JOIN customer_orders
  		ON runner_orders.order_id = customer_orders.order_id
  		WHERE distance <> ' '
		
-- Then, I applied the SUM and CASE functions:

		SELECT customer_id,
 		SUM(CASE 
  		WHEN exclusions <> ' ' OR extras <> ' ' THEN 1
  		ELSE 0
  		END) AS at_least_1_change,
 		SUM(CASE 
  		WHEN exclusions = ' ' AND extras = ' ' THEN 1 
  		ELSE 0
  		END) AS no_change
		FROM runner_orders
  		INNER JOIN customer_orders
  		ON runner_orders.order_id = customer_orders.order_id
  		WHERE distance <> ' '
  		GROUP BY customer_id
  
  **Question 8**: How many pizzas were delivered that had both exclusions and extras?
  
  
  		customer_id	exclusions_and_extras
		  104	                 1
  
  -- First, I joined the tables runner_orders and customer_orders, in order to select only the pizzas that had been delivered:

		SELECT dbo.runner_orders.order_id, customer_id, exclusions, extras
  		FROM runner_orders
  		INNER JOIN customer_orders
  		ON runner_orders.order_id = customer_orders.order_id
  		WHERE distance <> ' '
		

-- Then, I used the functions SUM, CASE and WHERE (to filter out the customers that wanted both exclusions):
  
  
  		SELECT customer_id,  
  		SUM, (CASE 
		WHEN exclusions <> ' ' AND extras <> ' ' THEN 1 
  		ELSE 0
  		END) AS exclusions_and_extras
  		FROM runner_orders
  		INNER JOIN customer_orders
  		ON runner_orders.order_id = customer_orders.order_id
  		WHERE distance <> ' '
  		AND exclusions <> ' ' 
  		AND extras <> ' '
  		GROUP BY customer_id
		

**Question 9**: What was the total volume of pizzas ordered for each hour of the day?

		hour_of_day	number_of_pizzas
		     13	                3
		     18	                3
		     19	                1
		     21	                2
		     23	                3
		

-- First, I joined the tables runner_orders and customer_orders with the information necessary for this table:

		SELECT dbo.runner_orders.order_id, customer_id, order_time
  		FROM runner_orders
  		INNER JOIN customer_orders
  		ON runner_orders.order_id = customer_orders.order_id
  		WHERE distance <> ' '
  
  
-- Using the function DATEPART to extract the time of the day:

		SELECT  DATEPART (HOUR, (order_time)) AS hour_of_day,
  		COUNT (dbo.runner_orders.order_id) AS number_of_pizzas
  		FROM runner_orders
  		INNER JOIN customer_orders
  		ON runner_orders.order_id = customer_orders.order_id
  		WHERE distance <> ' '
  		GROUP BY DATEPART (HOUR, (order_time))
		
		
**Question 10**: What was the volume of orders for each day of the week?

		day_of_the_week		number_of_pizzas
	 	    Saturday	                5
		    Thursday	                3
		    Wednesday	                4
		    
		    
-- Similar query as the question before, using DATENAME instead:

		SELECT  DATENAME (WEEKDAY, (order_time)) AS day_of_the_week,
		COUNT (*) AS number_of_pizzas
		FROM runner_orders
		INNER JOIN customer_orders
		ON runner_orders.order_id = customer_orders.order_id
		WHERE distance <> ' '
		GROUP BY DATENAME (WEEKDAY, (order_time))



### B. Runner and Customer Experience:

--Note: I realized that for some reason, the table customer_orders is in 2020, and the table runners is in 2021. I altered the table runners for 2020 in order to use both tables in the same year calendar:

		UPDATE dbo.runners
		SET registration_date = '2020-01-01'
		WHERE runner_id = 1

		UPDATE dbo.runners
		SET registration_date = '2020-01-03'
		WHERE runner_id = 2

		UPDATE dbo.runners
		SET registration_date = '2020-01-08'
		WHERE runner_id = 3

		UPDATE dbo.runners
		SET registration_date = '2020-01-15'
		WHERE runner_id = 4


**Question 1**: How many runners signed up for each 1 week period?


	week_period	number_of_runners
		1	       2
		2	       1
		3	       1


-- I used the function DATEPART for this question:

		SELECT  DATEPART (WEEK, (registration_date)) AS week_period,
  			COUNT (*) AS number_of_runners
  			FROM runners
  			GROUP BY DATEPART (WEEK, (registration_date))


**Question 2**: What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?


		runner_id	avg_runner_time
		    1	              15
		    2	              24
		    3	              10


-- First, I joined the tables customer_orders and runner_orders:

		SELECT dbo.customer_orders.order_id, runner_id, order_time, pickup_time
  		FROM customer_orders
  		INNER JOIN runner_orders
  		ON customer_orders.order_id = runner_orders.order_id
  		WHERE pickup_time <> ' '


-- Then, I calculated the time difference on a CTE using DATEDIFF function and then the average using AVG function:

		WITH cte AS (SELECT runner_id, DATEDIFF(MINUTE, order_time, pickup_time) AS time_difference
  		FROM customer_orders
  		INNER JOIN runner_orders
  		ON customer_orders.order_id = runner_orders.order_id
  		WHERE pickup_time <> ' ')

  		SELECT runner_id, AVG (time_difference) AS avg_runner_time
  		FROM cte
  		GROUP BY runner_id
		
		

**Question 3**: Is there any relationship between the number of pizzas and how long the order takes to prepare?


		order_id	number_of_orders	pickup_minutes
		   1	               1	              10
		   2	               1	              10
		   5	               1	              10
		   7	               1	              10
		   10	               2	              16
		   3	               2	              21
		   8	               1	              21
		   4	               3	              30

-- First, I joined the tables customer_orders and runner_orders:

		SELECT dbo.customer_orders.order_id, runner_id, order_time, pickup_time
  		FROM customer_orders
  		INNER JOIN runner_orders
  		ON customer_orders.order_id = runner_orders.order_id
  		WHERE pickup_time <> ' '


-- Then, I calculated the time difference on a CTE using DATEDIFF function and then the count of pizzas using COUNT function:


		WITH cte AS (SELECT dbo.customer_orders.order_id, DATEDIFF(MINUTE, order_time, pickup_time) AS pickup_minutes
			FROM customer_orders
			INNER JOIN runner_orders
			ON customer_orders.order_id = runner_orders.order_id
			WHERE pickup_time <> ' ')

			SELECT order_id, COUNT (order_id) AS number_of_orders, pickup_minutes
			FROM cte
			GROUP BY order_id, pickup_minutes
			
			
**Question 4**: What was the average distance travelled for each customer?


		customer_id	distance
		    101	         20.000000
		    102	         16.333333
		    103	         23.000000
		    104	         10.000000
		    105	         25.000000


-- Firts, I joined the tables "runner_orders" and customer_orders":


		SELECT dbo.customer_orders.order_id, customer_id, distance
  			FROM customer_orders
  			INNER JOIN runner_orders
  			ON customer_orders.order_id = runner_orders.order_id
  			WHERE pickup_time <> ' '
		
	
		
-- Then, I had to modify the column 'distance' into numbers, removing the "Km":

		UPDATE runner_orders
		SET distance = '20'
		WHERE order_id = 1


		UPDATE runner_orders
		SET distance = '20'
		WHERE order_id = 2

		UPDATE runner_orders
		SET distance = '13.4'
		WHERE order_id = 3

		UPDATE runner_orders
		SET distance = '25'
		WHERE order_id = 7

		UPDATE runner_orders
		SET distance = '23.4'
		WHERE order_id = 8

		UPDATE runner_orders
		SET distance = '10'
		WHERE order_id = 10

--Then, I calculuated the average distance using AVG and CAST to convert from VARCHAR into NUMERIC:

		SELECT customer_id, AVG(CAST (runner_orders.distance AS numeric)) AS distance
		FROM customer_orders
  		INNER JOIN runner_orders
  		ON customer_orders.order_id = runner_orders.order_id
  		WHERE pickup_time <> ' '
		GROUP BY customer_id
		

**Question 5**: What was the difference between the longest and shortest delivery times for all orders?

		delivery_time_difference
			30



-- First, I had to modify the column 'duration' into numbers:

		UPDATE runner_orders
  		SET duration = '32'
  		WHERE order_id = 1

  		UPDATE runner_orders
  		SET duration = '27'
  		WHERE order_id = 2

  		UPDATE runner_orders
  		SET duration = '20'
  		WHERE order_id = 3

  		UPDATE runner_orders
  		SET duration = '25'
  		WHERE order_id = 7

  		UPDATE runner_orders
  		SET duration = '15'
  		WHERE order_id = 8

  		UPDATE runner_orders
  		SET duration = '10'
  		WHERE order_id = 10

-- Then, I modified the column "duration" from VARCHAR to INT:

		ALTER TABLE runner_orders
  		ALTER COLUMN duration int


--Now, I calculated the difference using the functions MAX and MIN:

		SELECT 
    		MAX(duration) - MIN(duration) AS delivery_time_difference
		FROM runner_orders
		WHERE duration <> ' '
		
		

**Question 6**: What was the average speed for each runner for each delivery and do you notice any trend for these values?


	runner_id	customer_id	order_id	pizza_count	distance	avg_speed	day_of_week	Traffic
	1		101		1		1		20		37.50		Wednesday	Rush Hour
	1		101		2		1		20		44.44		Wednesday	Rush Hour
	1		102		3		2		13		39.00		Thursday	Late night
	1		104		10		2		10		60.00		Saturday	Rush Hour
	2		102		8		1		23		92.00		Thursday	Late night
	2		103		4		3		23		34.50		Saturday	Regular Traffic
	2		105		7		1		25		60.00		Wednesday	Late night
	3		104		5		1		10		40.00		Wednesday	Late night

-- Query using functions COUNT, CAST and ROUND:

		SELECT runner_id, customer_id, dbo.customer_orders.order_id, 
		COUNT(dbo.customer_orders.order_id) AS pizza_count, 
		CAST(distance AS numeric) AS distance,
		ROUND((CAST(distance AS numeric)/duration * 60), 2) AS avg_speed, 
		DATENAME (WEEKDAY, (order_time)) AS day_of_week,
		CASE WHEN 
		DATEPART (HOUR, (order_time)) BETWEEN 18 AND 20 THEN 'Rush Hour'
		WHEN DATEPART (HOUR, (order_time)) BETWEEN 21 AND 23 THEN 'Late night' 
		ELSE 'Regular Traffic'
		END AS Traffic
		FROM runner_orders 
		JOIN customer_orders 
		ON dbo.customer_orders.order_id = dbo.runner_orders.order_id
		WHERE distance <> ' '
		GROUP BY runner_id, customer_id, dbo.customer_orders.order_id, distance, duration, DATEPART (HOUR, (order_time)), DATENAME (WEEKDAY, (order_time))
		ORDER BY runner_id;
		
*TRENDS*:

*Runner 1*: Had the most orders. He seems to have a steady speed average, expect for one order number 10, when his average speed was up (since it was a Saturday, most likely he didn't hit much traffic)

*Runner 2*: Had the most discrepancy in the speed averages. 

*Runner 3*: Needs to pick up more orders for comparing. 


**Question 7**: What is the successful delivery percentage for each runner?

		runner_id	success_perc
		1		100
		2		75
		3		50
		
		
	SELECT runner_id, 
 	ROUND(100 * SUM
  	(CASE WHEN duration = 0 THEN 0
  	ELSE 1
  	END) / COUNT(*), 0) AS success_perc
	FROM runner_orders
	GROUP BY runner_id;












  

  
  
  
  
  
