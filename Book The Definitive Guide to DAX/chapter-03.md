# The Definitive Guide to DAX

## Chapter 3: Using basic table functions

<p>
  You cannot assign a table expression directly to a measure or to a calculated column.
</p>

```
=COUNTROWS(Sales)
```

<p>
  Every time you have a DAX function that accepts a table expression as an argument, you can write
  the name of a table in that parameter, or you can write a function that returns a table.
</p>

```
=COUNTROWS(
  FILTER(
    Sales,
    Sales[Unit Price] > 100
  )
)
```
### EVALUATE syntax

<p>
  You can use DAX both as a programming language and as a query language.
</p>

<p>
  The initial DEFINE MEASURE part can be useful to define measures thar are local to the query (that
  is, they exist for the lifetime of the query). It becomes very useful when you are debugging formulas, 
  because you can define a local measure, test it, and then put it in the model once it behaves as 
  expected.
</p>

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

<p>
  A typical use is in functions that iterate a table, computing a DAX expression 
  for each row.
</p>

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
