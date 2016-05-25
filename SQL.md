# SQL recap

## Selecting data

Select all data

`SELECT * FROM birdstrikes;`

Select all & limit

`SELECT * FROM birdstrikes LIMIT 10;`

Select certain fields

`SELECT bird_size, cost FROM birdstrikes LIMIT 10;`

## Ordering data

Order by a field

`SELECT state, cost FROM birdstrikes ORDER BY cost LIMIT 10;`

Order by a multiple fields

`SELECT state, cost FROM birdstrikes ORDER BY state, cost LIMIT 10;`

Order by a multiple fields

`SELECT state, cost FROM birdstrikes ORDER BY state, cost LIMIT 10;`

Reverse ordering

`SELECT state, cost FROM birdstrikes ORDER BY cost DESC;`

Reverse ordering by multple fields

`SELECT state, cost FROM birdstrikes ORDER BY state DESC, cost;`

## Renaming fields
`SELECT bird_size as size, state FROM birdstrikes`

```
SELECT
    state || ' - ' || flight_date as title,
    cost
FROM birdstrikes
ORDER BY cost DESC;
```

## Filtering data
Filter by field value

`SELECT * FROM birdstrikes WHERE state = 'Alabama';`

Filter by multiple conditions

`SELECT * FROM birdstrikes WHERE state = 'Alabama' AND bird_size = 'Small';`

`SELECT * FROM birdstrikes WHERE state = 'Alabama' OR state = 'Missouri';`

`SELECT * FROM birdstrikes WHERE state IN ('Alabama', 'Missouri');`

String operations:

`SELECT state FROM birdstrikes WHERE state LIKE 'A%' OR state LIKE '%a';`

`SELECT state FROM birdstrikes WHERE LOWER(state) = 'alabama';`

Lowercase: `LOWER`
Uppercase: `UPPER`

```
SELECT EXTRACT(DOW FROM flight_date) as day_of_week, * FROM birdstrikes
```

It can go complicated:

```
SELECT state, cost, bird_size FROM birdstrikes
WHERE (LOWER(state) = 'alabama'
        OR LOWER(state) = 'missoury')
      AND (bird_size = 'Small')
ORDER BY cost DESC;
```

## Removing duplicates
`SELECT DISTINCT state, size FROM birdstrikes;`

## WORKSHOP
* What's the maximum overall cost
* ^^ In which state did this accident happen?
* Display the first three states in alphabetical order?
* Display the bird sizes (don't display the "empty" cell)
* What is the size of the bird that caused the biggest damage in Missouri?

## Groupping and aggregation

COUNT(*)
```
SELECT COUNT(*) FROM birdstrikes;
```

Simple aggregations
```
SELECT MAX(cost) FROM birdstrikes
```

```
SELECT state, MAX(cost) AS max_cost FROM birdstrikes GROUP BY state ORDER BY state;
```

Multiple aggregate functions:
```
SELECT state, aircraft, COUNT(*), MAX(cost), MIN(cost), AVG(cost) FROM birdstrikes WHERE state LIKE 'A%' GROUP BY state, aircraft ORDER BY state, aircraft
```

**Sometimes it doesn't work**:
```
SELECT aircraft, state, MAX(cost) AS max_cost FROM birdstrikes GROUP BY state ORDER BY state;
```

Let's fix it:
```
SELECT state, aircraft, MAX(cost) AS max_cost FROM birdstrikes GROUP BY state, aircraft ORDER BY state, aircraft
```

You can filter here, too:
```
SELECT state, aircraft, MAX(cost) AS max_cost FROM birdstrikes WHERE state LIKE 'A%' GROUP BY state, aircraft ORDER BY state, aircraft
```

Advanced groupping - HAVING
```
SELECT state, COUNT(*) FROM birdstrikes GROUP BY state HAVING COUNT(*) > 100;
```

```
SELECT state, COUNT(*) FROM birdstrikes
WHERE state IS NOT NULL
GROUP BY state HAVING COUNT(*) > 100;
```

## WORKSHOP
 * Which is the state with the most accidents?
 * List the average (AVG) damage caused by each bird size?
 * List the maximum (MAX) damage caused by each bird size in each state.
 * ^^ Only show those rows from the above solutions which have a bigger damage than $100000
