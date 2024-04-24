# The Definitive Guide to DAX

## Chapter 3: Using basic table functions

You cannot assign a table expression directly to a measure or to a calculated column.

```
=COUNTROWS(Sales)
```

Every time you have a DAX function that accepts a table expression as an argument, you can write
the name of a table in that parameter, or you can write a function that returns a table.

```
=COUNTROWS(
  FILTER(
    Sales,
    Sales[Unit Price] > 100
  )
)
```

>If a table expression returns a table with one row and one column, a conversion to a scalar value is 
>possible and done automatically if required.

### EVALUATE syntax

You can use DAX both as a programming language and as a query language.
</br></br>
The initial `DEFINE MEASURE` part can be useful to define measures thar are local to the query (that
is, they exist for the lifetime of the query). It becomes very useful when you are debugging formulas, 
because you can define a local measure, test it, and then put it in the model once it behaves as 
expected.

```
[DEFINE { MEASURE <tableName>[<name>] = <expression> }]
EVALUATE <table>
[ORDER BY {<expression> [{ASC | DESC}]} [, ...]
  [START AT {<value|<parameter>} [, ...]] ]
```

```
EVALUATE Product
```

```
EVALUATE Product
ORDER BY
  Product[Color],
  Product[Brand] ASC,
  Product[Class] DESC
```

```
EVALUATE Product
ORDER BY
  Product[Color],
  Product[Brand] DESC,
  Product[Class] DESC
START AT
  "Yellow", "Tailspin Toys"
```

### Using table expressions

A typical use is in functions that iterate a table, computing a DAX expression 
for each row.

```
[Sales Amount] :=
SUMX(
  Sales,
  Sales[Quantity] * Sales[Unit Price]
)
```

```
[Sales Amount Multiple Items] :=
SUMX(
  FILTER(
    Sales,
    Sales[Quantity] > 1
  ),
  Sales[Quantity] * Sales[Unit Price]
)
```

```
Product[Product Sales Amount] =
SUMX(
  RELATEDTABLE(Sales),
  Sales[Quantity] * Sales[Unit Price]
)
```

```
Product[Product Sales Amount Multiple Items] =
SUMX(
  FILTER(
    RELATEDTABLE(Sales),
    Sales[Quantity] > 1
  ),
  Sales[Quantity] * Sales[Unit Price]
)
```

### Understanding FILTER

The `FILTER` function has a simple role: It gets a table and returns a table that has the same columns as
in the original table, but contains only the rows that satisfy a filter condition applied row by row.

```
FILTER(
  <table>,
  <condition>
)
```

`FILTER` iterates the "table" and, for each row, evaluated the "condition", which is a Boolean
expression. When the "condition" evaluates to `TRUE`, `FILTER` returns the row; otherwise, it skips it.

```
EVALUATE
FILTER(
  Product,
  Product[Brand] = "Fabrikam"
)
```

```
EVALUATE
FILTER(
  Product,
  AND(
    Product[Brand] = "Fabrikam",
    Product[Unit Price] > Product[Unit Cost] * 3
  )
)
```

```
EVALUATE
FILTER(
  FILTER(
    Product,
    Product[Unit Price] > Product[Unit Cost] * 3
  ),
  Product[Brand] = "Fabrikam"
)
```

```
EVALUATE
FILTER(
  FILTER(
    Product,
    Product[Brand] = "Fabrikam"
  ),
  Product[Unit Price] > Product[Unit Cost] * 3
)
```

You might choose the execution order to apply the most selective filter first.

### ALL function

`ALL` is a useful function that returns all the rows of a table or all the values of a column, depending
on the parameter you use.

```
EVALUATE
ALL(Product)
```

```
EVALUATE
ALL(Product[Class])
```

```
EVALUATE
ALL(
  Product[Class],
  Product[Color]
)
ORDER BY Product[Color]
```

In all of its variations, `ALL` ignores any existing filter to produce its result. You can use ALL as an 
argument of an iteration function, such as `SUMX` and `FILTER`, or as filter argument in a `CALCULATE`
function.

### ALLEXCEPT function

If you want to include most of the columns of a table in an ALL function call, you can use 
`ALLEXCEPT` instead. The syntax of `ALLEXCEPT` requires a table followed by the columns you want
to exclude from the result. As a result, `ALLEXCEPT` returns a table with a  unique list of existing
combinations of values in the other columns of the table

```
EVALUATE
ALLEXCEPT(
  Product,
  Product[ProductKey],
  Product[Color]
)
```

### ALLNOBLANKROW function

<p>
  When you call ALL on a parent table of a relationship, you retrieve an additional blank row if the 
  child table contains one or more rows that do not match any values in the parent  table. You can omit
  this special row from the result by using ALLNOBLANKROW instead of ALL.
</p>

```
[All NoBlank Brands] :=
COUNTROWS(
  ALLNOBLANKROW(Product[Brand])
)
```

You should use `ALLNOBLANKROW` only  when you  write a DAX formula that iterates values
ignoring unmatched values in relationships. However, the use of ALL is common, whereas
`ALLNOBLANKROW` is seldom used.

### VALUES function

`VALUES` returns the list of unique values that are visible in the current cell, including the optional
blank row for unmatched values.</br>
When there are no filters, the behavior of `VALUES` corresponds to `ALL`.</br>
`VALUES` also accepts a table as an argument. In that case, it returns the whole table that is visible in 
the current cell, optionally including the blank row for unmatched relationships.
</br></br>
Even if `VALUES` is a table function, you will often use it to compute scalar values.

```
[Color Name] :=
IF(
  COUNTROWS(VALUES(Product[Color])) = 1,
  VALUES(Product[Color])
)
```

### DISTINCT function

`DISTINCT` does the same as `VALUES`, without returning the blank row for unmatched values.</br>
When there are no filters, the behavior of `DISTINCT` corresponds to `ALLNOBLANKROW`.
