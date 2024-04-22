# The Definitive Guide to DAX

## Chapter 1: What is DAX?

### Using iterators
<p>
  An iterator does exactly what it name suggests: it iterates over a table and performs a calculation on each row of
  the table, aggregating the result to produce the single value you needed.
</p>

```
[AllSales] :=
SUMX(
  Sales, 
  Sales[ProductQuantity] * Sales[ProductPrice]
)
```

### Understanding relationship handling
<p>
  Even if you declared the relationship in the model using foreign keys, you still need to explicit
  and state the join condition in the query. Although it makes queries a little more verbose, this is useful
  because it lets you use different join conditions in different queries, giving you a lot of freedom in the 
  expressivity of the queries.
</p>

<p>
  In DAX, relationships are part of the model and they are all LEFT OUTER JOINS. Once defined in
  the model, you no longer need to specify the join type in the query: DAX uses an automatic LEFT
  OUTER JOIN in the query whenever you use columns related to the primary table. 
</p>

```
EVALUATE
SUMMARIZE(
  Sales,
  Customers[CustomerName],
  "SumOfSales", SUM(Sales[SalesAmount])
)
```

<p>
  Using DAX, you do not declare the WHERE condition in the query. Instead, you yse a specific
  function (FILTER) to filter the result:
</p>

```
EVALUATE
SUMMARIZE(
  FILTER(
    Customers,
    Customers[Continent] = "Europe"
  ),
  Customers[CustomerName],
  "SumOfSales", SUM(Sales[SalesAmount])
)
```

### Subqueries and conditions in DAX

<p>
  
</p>
  They naturally arise from the functional nature of the language.
</p>

```
EVALUATE
FILTER(
  SUMMARIZE(
    Customers,
    Customers[CustomerName],
    "SumOfSales", SUM(Sales[SalesAmount])
  ),
  [SumOfSales] > 100
)
```

### Hierarchies

<p>
  Using filter contexts and standard time intelligence functions.
</p>

```
[SamePeriodPreviousYearSales] :=
CALCULATE(
  SUM(Sales[SalesAmount]),
  SAMEPERIODLASTYEAR(Date[Date])
)
```

<p>
  If you want to clear a measure at the Year level, you need to check the presence of
  filters in the filter context.
</p>

```
[SamePeriodPreviousYearSales] :=
IF(
  ISFILTERED(Date[Month]),
  CALCULATE(
    SUM(Sales[Sales Amount]),
    SAMEPERIODLASTYEAR(Date[Date])
  ),
  BLANK()
)
```
