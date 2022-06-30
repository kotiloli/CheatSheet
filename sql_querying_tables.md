
# `IN` Operator Usage

```sql
--dvdrental
select customer_id, rental_id, return_date
from rental
where customer_id IN (10,12)
order by return_date DESC;

--dvdrental
select first_name, last_name
from customer
where customer_id IN (
    select customer_id
    from rental
    where CAST(return_date AS DATE) = '2005-05-27'
);
```

# `HAVING` Clause Usage

```sql
--show customers who spent 200 or more payment
select customer_id, SUM(amount)
from payment
group by customer_id
HAVING sum(amount) > 200;

--show total amount of customer 526
select sum(amount)
from payment 
where customer_id = 526;
```

# `GROUP BY` Clause Usage

```sql
--show each customers total amount till date
select customer_id, SUM(amount)
from payment
group by customer_id
order by SUM(amount) ASC;

--show the stores which have 200 or more customers
select store_id, COUNT(customer_id)
from customer
GROUP BY store_id
HAVING COUNT(customer_id) > 200;

--show how many payment transaction has taken per staff 
--member
select staff_id, COUNT(payment_id)
from payment
group by staff_id;
```

# `LIKE` Operator Usage

```sql

-- list people of whose name starts with 'K'
-- and the third character is 'i'
select first_name, last_name
from customer
where first_name LIKE 'K_i%';
```

# `BETWEEN` Operator Usage

```sql
-- list payments whose amount between 3 and 5 USD
SELECT customer_id, payment_id, amount
FROM payment
WHERE amount BETWEEN 3 AND 5;

--show  the payments whose payment date is 
--between 2007-02-07 and 2007-02-15 
SELECT customer_id, payment_id, amount, payment_date
FROM payment
WHERE payment_date BETWEEN '2007-02-07' AND '2007-02-15';
--Note: While making date queries the literal date in 
--ISO 8601 format i.e., YYYY-MM-DD should be used in 
--PostgreSQL.

```
