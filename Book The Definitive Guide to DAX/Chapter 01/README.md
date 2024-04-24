# The Definitive Guide to DAX

## Chapter 1: What is DAX?
DAX is the programming language of Microsoft SQL Server Analysis Services (SSAS) and Microsoft
Power Pivot for Excel. It was created in 2010, with the first release of the PowerPivot for Excel 2010.
Over time, DAX gained popularity in the Excel community, which uses DAX to create Power Pivot data models
in Excel, and in the Business Intelligence (BI) community, which uses DAX to build models with SSAS.

### Using iterators
An iterator does exactly what it name suggests: it iterates over a table and performs a calculation on each row of
the table, aggregating the result to produce the single value you needed.

```
[AllSales] :=
SUMX(
  Sales, 
  Sales[ProductQuantity] * Sales[ProductPrice]
)
```

### Understanding relationship handling
Even if you declared the relationship in the model using foreign keys, you still need to explicit
and state the join condition in the query. Although it makes queries a little more verbose, this is useful
because it lets you use different join conditions in different queries, giving you a lot of freedom in the 
expressivity of the queries.
</br></br>
In DAX, relationships are part of the model and they are all LEFT OUTER JOINS. Once defined in
the model, you no longer need to specify the join type in the query: DAX uses an automatic LEFT
OUTER JOIN in the query whenever you use columns related to the primary table. 

```
EVALUATE
SUMMARIZE(
  Sales,
  Customers[CustomerName],
  "SumOfSales", SUM(Sales[SalesAmount])
)
```
Using DAX, you do not declare the WHERE condition in the query. Instead, you yse a specific function `FILTER` to filter the result:

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

They naturally arise from the functional nature of the language.

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

Using filter contexts and standard time intelligence functions.

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
