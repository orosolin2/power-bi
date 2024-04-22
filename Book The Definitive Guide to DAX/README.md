# Livro "The Definitive Guide to DAX" dos autores Marco Russo e Alberto Ferrari, publicado em 2015 pela Microsoft Press.

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
