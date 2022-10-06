# SQL-Exercise-Solutions

## SELECT (learning stage)

1. Condition – Find the model number, speed and hard drive capacity for all the PCs with prices below $500.

Execution:

~~~
SELECT 
  model, 
  speed, 
  hd 
FROM 
  pc 
WHERE 
  price < 500
~~~

Result set: model, speed and hd.

2. Condition – List all printer makers.

Execution:

~~~
SELECT 
  DISTINCT maker 
FROM 
  product 
WHERE 
  type = 'Printer'
~~~

Result set: maker.

3. Condition – Find the model number, RAM and screen size of the laptops with prices over $1000.

Execution:

~~~
SELECT 
  model, 
  ram, 
  screen 
FROM 
  laptop 
WHERE 
  price > 1000
~~~

Result set: model, ram and screen.

4. Condition – Find all records from the Printer table containing data about color printers.

Execution:

~~~
SELECT 
  * 
FROM 
  printer 
WHERE 
  color = 'y'
~~~

Result set: code, model, color, type and price.

5. Condition – Find the model number, speed and hard drive capacity of PCs cheaper than $600 having a 12x or a 24x CD drive.

Execution:

~~~
SELECT 
  model, 
  speed, 
  hd 
FROM 
  pc 
WHERE 
  price < 600 
  AND (
    cd = '12x' 
    OR cd = '24x'
  )
~~~

Result set: model, speed and hd.

6. Condition – For each maker producing laptops with a hard drive capacity of 10 Gb or higher, find the speed of such laptops.

Execution:

~~~
SELECT 
  DISTINCT product.maker, 
  laptop.speed 
FROM 
  product 
  INNER JOIN laptop ON product.model = laptop.model 
WHERE 
  laptop.hd >= 10
~~~

Result set: maker and speed.

7. Condition – Get the models and prices for all commercially available products (of any type) produced by maker B.

Execution:

~~~
SELECT 
  p.model, 
  pc.price 
FROM 
  pc 
  INNER JOIN product p ON pc.model = p.model 
WHERE 
  maker = 'B' 
UNION 
SELECT 
  p.model, 
  pr.price 
FROM 
  printer pr 
  INNER JOIN product p ON pr.model = p.model 
WHERE 
  maker = 'B' 
UNION 
SELECT 
  p.model, 
  lp.price 
FROM 
  laptop lp 
  INNER JOIN product p ON lp.model = p.model 
WHERE 
  maker = 'B'
~~~

Result set: model and price.

8. Condition – Find the makers producing PCs but not laptops.

Execution:

~~~
SELECT 
  DISTINCT maker 
FROM 
  product 
WHERE 
  type = 'PC' 
  AND maker NOT IN (
    SELECT 
      maker 
    FROM 
      product 
    WHERE 
      type = 'Laptop'
  )
~~~

Result set: maker.

9. Condition – Find the makers of PCs with a processor speed of 450 MHz or more.

Execution:

~~~
SELECT 
  DISTINCT product.maker 
FROM 
  product 
  INNER JOIN pc ON product.model = pc.model 
WHERE 
  speed >= 450
~~~

Result set: maker.

10. Condition – Find the printer models having the highest price.

Execution:

~~~
SELECT 
  model, 
  price 
FROM 
  printer 
WHERE 
  price = (
    SELECT 
      MAX(price) 
    FROM 
      printer
  )
~~~

Result set: model and price.

11.	Condition – Find out the average speed of PCs.

Execution:

~~~
SELECT 
  AVG(speed) AS Avg_speed 
FROM 
  pc
~~~

Result set: Avg_speed.

12.	Condition – Find out the average speed of the laptops priced over $1000.

Execution:

~~~
SELECT 
  AVG(speed) AS Avg_speed 
FROM 
  laptop 
WHERE 
  price > 1000
~~~

Result set: Avg_speed.

13.	Condition – Find out the average speed of the PCs produced by maker A.

Execution:

~~~
SELECT 
  AVG(speed) AS Avg_speed 
FROM 
  pc 
  INNER JOIN product ON pc.model = product.model 
WHERE 
  maker = 'A'
~~~

Result set: Avg_speed.

14.	Condition – For the ships in the Ships table that have at least 10 guns, get the class, name, and country.

Execution:

~~~
SELECT 
  ships.class, 
  ships.name, 
  classes.country 
FROM 
  ships 
  INNER JOIN classes ON ships.class = classes.class 
WHERE 
  numGuns >= 10
~~~

Result set: class, name and country.

15.	Condition – Get hard drive capacities that are identical for two or more PCs.

Execution:

~~~
SELECT 
  DISTINCT hd 
FROM 
  PC 
GROUP BY 
  hd 
HAVING 
  COUNT(hd) > 1 
ORDER BY 
  hd ASC
~~~

Result set: hd.

16.	Condition – Get pairs of PC models with identical speeds and the same RAM capacity. Each resulting pair should be displayed only once, i.e. (i, j) but not (j, i).

Execution:

~~~
SELECT 
  DISTINCT pc1.model, 
  pc2.model, 
  pc1.speed, 
  pc1.ram 
FROM 
  pc pc1 
  JOIN pc pc2 ON pc1.model > pc2.model 
  AND pc1.speed = pc2.speed 
  AND pc1.ram = pc2.ram
~~~

Result set: model with the bigger number, model with the smaller number, speed and RAM.

17.	Condition – Get the laptop models that have a speed smaller than the speed of any PC.

Execution:

~~~
SELECT 
  DISTINCT type, 
  laptop.model, 
  speed 
FROM 
  laptop 
  INNER JOIN product ON laptop.model = product.model 
WHERE 
  speed < ALL (
    SELECT 
      DISTINCT speed 
    FROM 
      pc
  )
~~~

Result set: type, model and speed.

18.	Condition – Find the makers of the cheapest color printers.

Execution:

~~~
SELECT 
  DISTINCT maker, 
  price 
FROM 
  printer 
  INNER JOIN product ON printer.model = product.model 
WHERE 
  color = 'y' 
  AND product.type = 'Printer' 
  AND price = (
    SELECT 
      MIN(price) 
    FROM 
      printer 
    WHERE 
      color = 'y'
  )
~~~

Result set: maker and price.

19.	Condition – For each maker having models in the Laptop table, find out the average screen size of the laptops he produces.

Execution:

~~~
SELECT 
  product.maker, 
  AVG(screen) AS Avg_screen 
FROM 
  laptop 
  INNER JOIN product ON laptop.model = product.model 
GROUP BY 
  maker
~~~

Result set: maker and average screen size.

20.	Condition – Find the makers producing at least three distinct models of PCs.

Execution:

~~~
SELECT 
  maker, 
  COUNT(model) 
FROM 
  product 
WHERE 
  type = 'PC' 
GROUP BY 
  maker 
HAVING 
  COUNT(model) >= 3
~~~

Result set: maker and number of PC models.

21.	Condition – Find out the maximum PC price for each maker having models in the PC table.

Execution:

~~~
SELECT 
  maker, 
  MAX(price) AS 'Max_price' 
FROM 
  pc 
  INNER JOIN product ON pc.model = product.model 
GROUP BY 
  maker
~~~

Result set: maker and maximum price.

22.	Condition – For each value of PC speed that exceeds 600 MHz, find out the average price of PCs with identical speeds.

Execution:

~~~
SELECT 
  speed, 
  AVG(price) AS 'Avg_price' 
FROM 
  pc 
WHERE 
  speed > 600 
GROUP BY 
  speed
~~~

Result set: speed and average price.

23.	Condition – Get the makers producing both PCs having a speed of 750 MHz or higher and laptops with a speed of 750 MHz or higher.

Execution:

~~~
SELECT 
  DISTINCT maker 
FROM 
  product, 
  pc 
WHERE 
  product.model = pc.model 
  AND speed >= 750 
  AND maker IN (
    SELECT 
      DISTINCT maker 
    FROM 
      product, 
      laptop 
    WHERE 
      product.model = laptop.model 
      AND speed >= 750
  )
~~~

Result set: maker.

24.	Condition – List the models of any type having the highest price of all products present in the database.

Execution:

~~~
WITH cte AS (
  SELECT 
    model, 
    price 
  FROM 
    pc 
  UNION 
  SELECT 
    model, 
    price 
  FROM 
    laptop 
  UNION 
  SELECT 
    model, 
    price 
  FROM 
    printer
) 
SELECT 
  model 
FROM 
  cte 
WHERE 
  price = (
    SELECT 
      MAX(price) 
    FROM 
      cte
  )
~~~

Result set: model.

25.	Condition – Find the printer makers also producing PCs with the lowest RAM capacity and the highest processor speed of all PCs having the lowest RAM capacity.

Execution:

~~~
SELECT 
  DISTINCT maker 
FROM 
  product 
  JOIN pc ON product.model = pc.model 
WHERE 
  ram = (
    SELECT 
      MIN(ram) 
    FROM 
      pc
  ) 
  AND code IN (
    SELECT 
      code 
    FROM 
      (
        SELECT 
          code, 
          model, 
          speed, 
          ram 
        FROM 
          pc 
        WHERE 
          ram = (
            SELECT 
              MIN(ram) 
            FROM 
              pc
          )
      ) a 
    WHERE 
      speed = (
        SELECT 
          MAX(speed) 
        FROM 
          pc 
        WHERE 
          ram = (
            SELECT 
              MIN(ram) 
            FROM 
              pc
          )
      )
  ) 
  AND maker IN (
    SELECT 
      maker 
    FROM 
      product 
    WHERE 
      type = 'printer'
  )
~~~

Result set: maker.

26.	Condition – Find out the average price of PCs and laptops produced by maker A..

Execution:

~~~
SELECT 
  (
    SELECT 
      AVG(price) 
    FROM 
      (
        SELECT 
          price 
        FROM 
          pc 
          INNER JOIN product ON product.model = pc.model 
        WHERE 
          maker = 'A' 
        UNION ALL 
        SELECT 
          price 
        FROM 
          laptop 
          INNER JOIN product ON product.model = laptop.model 
        WHERE 
          maker = 'A'
      ) AS price
  ) AS Avg_price
~~~

Result set: one overall average price for all items.

27.	Condition – Find out the average hard disk drive capacity of PCs produced by makers who also manufacture printers.

Execution:

~~~
SELECT 
  maker, 
  AVG(hd) AS 'Avg_hd' 
FROM 
  product 
  JOIN pc ON product.model = pc.model 
WHERE 
  maker IN (
    SELECT 
      maker 
    FROM 
      product 
    WHERE 
      type = 'printer'
  ) 
GROUP BY 
  maker
~~~

Result set: maker and average HDD capacity.

28.	Condition – Using Product table, find out the number of makers who produce only one model.

Execution:

~~~
SELECT 
  COUNT(maker) AS 'qty' 
FROM 
  (
    SELECT 
      maker, 
      COUNT(model) AS count 
    FROM 
      product 
    GROUP BY 
      maker
  ) a 
WHERE 
  count = 1
~~~

Result set: qty.

29.	Condition – Under the assumption that receipts of money (inc) and payouts (out) are registered not more than once a day for each collection point [i.e. the primary key consists of (point, date)], write a query displaying cash flow data (point, date, income, expense).

Execution:

~~~
SELECT 
  CASE WHEN Income_o.point IS NULL THEN Outcome_o.point ELSE Income_o.point END AS point, 
  CASE WHEN Income_o.date IS NULL THEN Outcome_o.date ELSE Income_o.date END AS date, 
  inc AS income, 
  out AS expense 
FROM 
  Income_o FULL 
  JOIN Outcome_o ON Income_o.point = Outcome_o.point 
  AND Income_o.date = Outcome_o.date 
ORDER BY 
  date
~~~

Result set: point, date, income and outcome.

30.	Condition – Under the assumption that receipts of money (inc) and payouts (out) can be registered any number of times a day for each collection point [i.e. the code column is the primary key], display a table with one corresponding row for each operating date of each collection point.

Execution:

~~~
SELECT 
  CASE WHEN a.point IS NULL THEN b.point ELSE a.point END AS point, 
  CASE WHEN a.date IS NULL THEN b.date ELSE a.date END AS date, 
  outcome, 
  income 
FROM 
  (
    SELECT 
      point, 
      date, 
      SUM(out) as outcome 
    FROM 
      outcome 
    GROUP BY 
      point, 
      date
  ) a FULL 
  JOIN (
    SELECT 
      point, 
      date, 
      SUM(inc) as income 
    FROM 
      income 
    GROUP BY 
      point, 
      date
  ) b ON b.date = a.date 
  AND b.point = a.point 
ORDER BY 
  date
~~~

Result set: point, date, total payout per day (out) and total money intake per day (inc). Missing values are considered to be NULL.

## SELECT (rating stages)

1. Condition – Dima and Misha use products by the same maker. The type of Tanya's printer differs from the Vitya’s one, but their color properties have the same value. 
The screen of Dima's laptop is 3 inches bigger than the screen of Olya's one. Misha's PC is 4 times more expensive than Tanya's printer. The model numbers of Vitya's printer and Olya's laptop differ by the third character only. Kostya's PC has a processor speed like Misha's PC; a hard drive capacity like Dima's laptop; a RAM capacity like Olya's laptop; and costs like Vitya's printer.

Execution:

~~~
SELECT 
  DISTINCT model 
FROM 
  pc 
  INNER JOIN (
    SELECT 
      laptop.hd, 
      x.speed, 
      x.price, 
      x.ram 
    FROM 
      laptop 
      INNER JOIN (
        SELECT 
          DISTINCT maker, 
          pc.speed, 
          x.screen, 
          x.price, 
          x.ram 
        FROM 
          printer p 
          INNER JOIN (
            SELECT 
              DISTINCT p.price, 
              p.color, 
              p.type, 
              l.screen, 
              l.ram 
            FROM 
              printer p 
              /*Viktor’s printer and Olga’s Laptop*/
              CROSS 
              JOIN laptop l 
            WHERE 
              SUBSTRING(p.model, 1, 2) = SUBSTRING(l.model, 1, 2) 
              AND (
                SUBSTRING(
                  p.model, 
                  4, 
                  LEN(p.model)
                ) = SUBSTRING(
                  l.model, 
                  4, 
                  LEN(l.model)
                )
              ) 
              AND (
                SUBSTRING(p.model, 3, 1) <> SUBSTRING(l.model, 3, 1) 
                AND LEN(l.model) > 2 
                AND LEN(p.model) > 2
              )
          ) x ON x.color = p.color 
          AND x.type <> p.type 
          /*Tanya’s printer*/
          INNER JOIN pc ON pc.price = p.price * 4 
          INNER JOIN product prod ON prod.model = pc.model
      ) x ON laptop.screen = x.screen + 3 
      AND laptop.model IN (
        SELECT 
          model 
        FROM 
          product 
        WHERE 
          maker = x.maker
      )
  ) x ON pc.speed = x.speed 
  AND pc.hd = x.hd 
  AND pc.ram = x.ram 
  AND pc.price = x.price
~~~

Result set: Display all possible model numbers for Kostya's PC.

2. Condition – For each month (with regards to year) present in the Battles table, find out the number of occurrences for each day of the week.

Execution:

~~~
SELECT 
  FORMAT(date, 'yyyy-MM') AS 'm', 
  DATEDIFF(
    WEEK, 
    DATEADD(
      DAY, 
      - (1 + 1) % 7, 
      FORMAT(date, 'yyyy-MM') + '-01'
    ), 
    DATEADD(
      DAY, 
      - (1 + 1) % 7, 
      DATEADD(
        MONTH, 
        1, 
        FORMAT(date, 'yyyy-MM') + '-01'
      )
    )
  ) AS 'Mon', 
  DATEDIFF(
    WEEK, 
    DATEADD(
      DAY, 
      - (2 + 1) % 7, 
      FORMAT(date, 'yyyy-MM') + '-01'
    ), 
    DATEADD(
      DAY, 
      - (2 + 1) % 7, 
      DATEADD(
        MONTH, 
        1, 
        FORMAT(date, 'yyyy-MM') + '-01'
      )
    )
  ) AS 'Tue', 
  DATEDIFF(
    WEEK, 
    DATEADD(
      DAY, 
      - (3 + 1) % 7, 
      FORMAT(date, 'yyyy-MM') + '-01'
    ), 
    DATEADD(
      DAY, 
      - (3 + 1) % 7, 
      DATEADD(
        MONTH, 
        1, 
        FORMAT(date, 'yyyy-MM') + '-01'
      )
    )
  ) AS 'Wed', 
  DATEDIFF(
    WEEK, 
    DATEADD(
      DAY, 
      - (4 + 1) % 7, 
      FORMAT(date, 'yyyy-MM') + '-01'
    ), 
    DATEADD(
      DAY, 
      - (4 + 1) % 7, 
      DATEADD(
        MONTH, 
        1, 
        FORMAT(date, 'yyyy-MM') + '-01'
      )
    )
  ) AS 'Thu', 
  DATEDIFF(
    WEEK, 
    DATEADD(
      DAY, 
      - (5 + 1) % 7, 
      FORMAT(date, 'yyyy-MM') + '-01'
    ), 
    DATEADD(
      DAY, 
      - (5 + 1) % 7, 
      DATEADD(
        MONTH, 
        1, 
        FORMAT(date, 'yyyy-MM') + '-01'
      )
    )
  ) AS 'Fri', 
  DATEDIFF(
    WEEK, 
    DATEADD(
      DAY, 
      - (6 + 1) % 7, 
      FORMAT(date, 'yyyy-MM') + '-01'
    ), 
    DATEADD(
      DAY, 
      - (6 + 1) % 7, 
      DATEADD(
        MONTH, 
        1, 
        FORMAT(date, 'yyyy-MM') + '-01'
      )
    )
  ) AS 'Sat', 
  DATEDIFF(
    WEEK, 
    DATEADD(
      DAY, 
      - (7 + 1) % 7, 
      FORMAT(date, 'yyyy-MM') + '-01'
    ), 
    DATEADD(
      DAY, 
      - (7 + 1) % 7, 
      DATEADD(
        MONTH, 
        1, 
        FORMAT(date, 'yyyy-MM') + '-01'
      )
    )
  ) AS 'Sun' 
FROM 
  battles 
GROUP BY 
  FORMAT(date, 'yyyy-MM')
~~~

Result set: month (in "YYYY-MM" format) and number of Mondays, Tuesdays, ...Sundays.

3. Condition – For the Product table, get a result set consisting of the following columns: maker, pc, laptop, and printer. For each maker, it should be indicated whether he manufacturers goods of a certain type (if so, the corresponding column should contain "yes"), or not ("no"). In the first case, "yes" should be directly followed (without any spaces) by the number of distinct models of the corresponding product type offered for sale (that is, present in the PC, Laptop, or Printer table) enclosed in parentheses.

Execution:

~~~
SELECT 
  DISTINCT a.maker, 
  CASE WHEN a.maker IN (
    SELECT 
      pr.maker 
    FROM 
      product AS pr 
      LEFT JOIN pc pc_a ON pr.model = pc_a.model 
    WHERE 
      pr.type = 'pc'
  ) THEN 'yes(' + CAST(
    (
      SELECT 
        COUNT(DISTINCT b.model) 
      FROM 
        pc b, 
        product c 
      WHERE 
        b.model = c.model 
        AND c.type = 'pc' 
        AND c.maker = a.maker
    ) AS VARCHAR
  ) + ')' ELSE 'no' END AS pc, 
  CASE WHEN a.maker IN (
    SELECT 
      pr.maker 
    FROM 
      product as pr 
      LEFT JOIN laptop lp_a ON pr.model = lp_a.model 
    WHERE 
      pr.type = 'laptop'
  ) THEN 'yes(' + CAST(
    (
      SELECT 
        COUNT(DISTINCT b.model) 
      FROM 
        laptop b, 
        product c 
      WHERE 
        b.model = c.model 
        AND c.type = 'laptop' 
        AND c.maker = a.maker
    ) AS VARCHAR
  ) + ')' ELSE 'no' END AS laptop, 
  CASE WHEN a.maker IN (
    SELECT 
      pr.maker 
    FROM 
      product AS pr 
      LEFT JOIN printer print_a ON pr.model = print_a.model 
    WHERE 
      pr.type = 'printer'
  ) THEN 'yes(' + CAST(
    (
      SELECT 
        COUNT(DISTINCT b.model) 
      FROM 
        printer b, 
        product c 
      WHERE 
        b.model = c.model 
        AND c.type = 'printer' 
        AND c.maker = a.maker
    ) AS VARCHAR
  ) + ')' ELSE 'no' END AS printer 
FROM 
  product a
~~~

Result set: maker, pc, laptop and printer.

4. Condition – Calculate the sum of digits in each model number (model column) in the Product table.

Execution:

~~~
SELECT 
  model, 
  (
    DATALENGTH(model) - DATALENGTH(
      REPLACE(model, '1', '')
    )
  ) * 1 + (
    DATALENGTH(model) - DATALENGTH(
      REPLACE(model, '2', '')
    )
  ) * 2 + (
    DATALENGTH(model) - DATALENGTH(
      REPLACE(model, '3', '')
    )
  ) * 3 + (
    DATALENGTH(model) - DATALENGTH(
      REPLACE(model, '4', '')
    )
  ) * 4 + (
    DATALENGTH(model) - DATALENGTH(
      REPLACE(model, '5', '')
    )
  ) * 5 + (
    DATALENGTH(model) - DATALENGTH(
      REPLACE(model, '6', '')
    )
  ) * 6 + (
    DATALENGTH(model) - DATALENGTH(
      REPLACE(model, '7', '')
    )
  ) * 7 + (
    DATALENGTH(model) - DATALENGTH(
      REPLACE(model, '8', '')
    )
  ) * 8 + (
    DATALENGTH(model) - DATALENGTH(
      REPLACE(model, '9', '')
    )
  ) * 9 AS 'qty' 
FROM 
  product 
ORDER BY 
  model
~~~

Result set: model and sum of digits.

5. Condition – Output all records from the Outcome and Income tables whose dates are at least two calendar months away from the maximum date in both these tables (e. g., if the maximum date is 2009-12-05 the latest date to be displayed has to be before 2009-10-01). Arrange these records by month, assigning a consecutive number to each month (with regard to year) occurring in the result set.

Execution:

~~~
WITH cte AS (
  SELECT 
    code, 
    point, 
    date, 
    inc sum 
  FROM 
    income 
  UNION ALL 
  SELECT 
    code, 
    point, 
    date, 
    - out 
  FROM 
    outcome
) 
SELECT 
  DENSE_RANK() OVER(
    ORDER BY 
      DATEDIFF(mm, 0, date)
  ) num, 
  REPLACE(
    CONVERT(
      varchar, 
      DATEADD(
        month, 
        DATEDIFF(month, 0, date), 
        0
      ), 
      102
    ), 
    '.', 
    '-'
  ) startDate, 
  REPLACE(
    CONVERT(
      varchar, 
      DATEADD(
        dd, 
        -1, 
        DATEADD(
          month, 
          DATEDIFF(month, 0, date) + 1, 
          0
        )
      ), 
      102
    ), 
    '.', 
    '-'
  ) endDate, 
  code, 
  point, 
  date, 
  sum 
FROM 
  cte 
WHERE 
  date < DATEADD(
    month, 
    DATEDIFF(
      month, 
      0, 
      (
        SELECT 
          MAX(date) 
        FROM 
          cte
      )
    ) -2, 
    0
  )
~~~

Result set: consecutive number of the month, first day of the month in "yyyy-mm-dd" format, last day of the month in "yyyy-mm-dd" format, code, point, date, amount of money (should be negative for the Outcome table).
