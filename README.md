# Module_7

# Data Analysis
## Part 1:
    *The CFO of your firm has requested a report to help analyze potential fraudulent transactions. Using your newly created database, generate queries that will discover the information needed to answer the following questions, then use your repository's ReadME file to create a markdown report you can share with the CFO:*

### Some fraudsters hack a credit card by making several small transactions (generally less than $2.00), which are typically ignored by cardholders.

* How can you isolate (or group) the transactions of each cardholder?
   
    CREATE VIEW transactions_by_cardholder AS
    SELECT credit_card.cardholder_id, transaction.date, transaction.amount
    FROM credit_card
    INNER JOIN transaction ON credit_card.card = transaction.card
    ORDER BY credit_card.cardholder_id; 

* Count the transactions that are less than $2.00 per cardholder.
    
    CREATE VIEW num_transactions_under_2_by_cardholder AS
    SELECT credit_card.cardholder_id, COUNT(transaction.id) as number_transactions
    FROM credit_card
    INNER JOIN transaction ON credit_card.card = transaction.card
    WHERE transaction.amount < 2.00
    GROUP BY credit_card.cardholder_id;

* Is there any evidence to suggest that a credit card has been hacked? Explain your rationale.
    
    With the data provided through the two methods above we can see that there are a decent amount of transactions below $2.00 indicating that the card is compromised. 

### Take your investigation a step further by considering the time period in which potentially fraudulent transactions are made.
* What are the top 100 highest transactions made between 7:00 am and 9:00 am?
    
    CREATE VIEW highest_transactions_7_to_9 AS
    SELECT *
    FROM transaction
    WHERE EXTRACT(HOUR FROM transaction.date) >= 7
    AND EXTRACT(HOUR FROM transaction.date) < 9
    ORDER BY transaction.amount DESC
    LIMIT 100;

* Do you see any anomalous transactions that could be fraudulent?
    
    When comparing the two highest transactions inside 7-9am and outside of 7-9am I noticed inside of the 7-9am that the purchases throughout the morning have 7 high transactions and the rest of them are small. Compared to outside of 7-9am you can see that most if not all the transactions are well into the hundreds. Since they have so many transactions they could high all the small transaction amounts in the morning when nobody is paying attention and when the bigger amounts come in later the small amounts are hidden away. 

* Is there a higher number of fraudulent transactions made during this time frame versus the rest of the day?
    
    I believe there is a higher amount of fraudulent transcations made in the morning. 

* If you answered yes to the previous question, explain why you think there might be fraudulent transactions during this time frame. 

    I think transactions may occur during these times when no one is around to catch them being made. 

* What are the top 5 merchants prone to being hacked using small transactions?

    1. Wood-Ramirez
    2. Hood-Philips
    3. Baker Inc
    4. Clark and Sons
    5. Greene-Wood

* Create a view for each of your queries.
    
    CREATE VIEW num_small_transactions_by_merchant AS
    SELECT merchant.name, COUNT(transaction.id) as number_transactions
    FROM merchant
    INNER JOIN transaction ON merchant.id = transaction.id_merchant
    WHERE transaction.amount < 2.00
    GROUP BY merchant.id
    ORDER BY number_transactions DESC
    LIMIT 5;

## Part 2
*Your CFO has also requested detailed trends data on specific card holders. Use the starter notebook to query your database and generate visualizations that supply the requested information as follows, then add your visualizations and observations to your markdown report:*

The two most important customers of the firm may have been hacked. Verify if there are any fraudulent transactions in their history. For privacy reasons, you only know that their cardholder IDs are 2 and 18.
Using hvPlot, create a line plot representing the time series of transactions over the course of the year for each cardholder separately.

Next, to better compare their patterns, create a single line plot that contains both card holders' trend data.

* What difference do you observe between the consumption patterns? Does the difference suggest a fraudulent transaction? Explain your rationale. 
    By observing the transactions of cardholder 2 we can see that there are small transactions performed of $.70 to $2 when the avg transcation is about $10+. We can conclued that there are fraudulent transcatons being performed. As for cardholder 18 we see that there 8 large purchases made for their specific card of $300+. The biggest difference between the two graphs shows us how scammers may use small vs large transactions to steal money. 

The CEO of the biggest customer of the firm suspects that someone has used her corporate credit card without authorization in the first quarter of 2018 to pay quite expensive restaurant bills. Again, for privacy reasons, you know only that the cardholder ID in question is 25.

Using hvPlot, create a box plot, representing the expenditure data from January 2018 to June 2018 for cardholder ID 25.

* Are there any outliers for cardholder ID 25? How many outliers are there per month?
    There are a few (3) outliners throughout the months besides Feb. 
* Do you notice any anomalies? Describe your observations and conclusions.
    It is hard to say if these ca be counted as anything and would need to be dived into to get a better answer. 