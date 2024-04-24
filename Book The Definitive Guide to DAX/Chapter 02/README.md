# The Definitive Guide to DAX

## Chapter 2: Introducing DAX

### Calculated columns

A calculated column is just like any other column in a table and you can use it in rows, columns, 
filters, or values of a pivot table or any other report. You can also a calculated column to define a 
relationship, if needed. The DAX expression defined for a calculated column operates in the context of 
the current row of the table to which it belongs. Any reference to a column returns the value of that
column for the current row. You canot directly access the values of other rows.
</br></br>
Calculated columns are computed during the database processing and then stored in the model. The time
required to compute them is always process time and not query time, resulting in a better user experience.
Nevertheless, you always must remember that calculated column uses precious RAM.

### Measures

A measure is a calculation that aggregate values from many rows in a table. A measure operates on 
aggregations of data defined by the current context.

### Variables

To avoid repeating the same expression. They are local to the expression in which you define them.
Variables are computed using lazy evaluation

```
VAR
  TotalSales = SUM(Sales[SalesAmount])
RETURN
  (TotalSales - SUM(Sales[TotalProductCost])) / TotalSales
```

### Empty or missing values

DAX handles missing values, blank values, or empty cells in the same way, using the value `BLANK`.
</br>
`BLANK` is not a real value but a special way to identify these conditions.

### Logical functions

#### SWITCH

Internally, DAX translates `SWITCH` statements into  a set of nested `IF` functions.

```
SizeDesc =
  SWITCH(
    Product[Size], 
    "S", "Small",
    "M", "Medium",
    "L", "Large",
    "XL", "Extra Large",
    "Other"
  )
```

```
SWITCH(
  TRUE(),
  Product[Size] = "XL" && Product[Color] = "Red", "Red and XL",
  Product[Size] = "XL" && Product[Color] = "Blue", "Blue and XL",
  Product[Size] = "L" & Product[Color] = "Green", "Green and L"
)
```

### Relational functions

#### RELATED

`RELATED` function enables you to access columns in the related table. It does not matter how 
many steps are necessary to travel from the original table to the related one,  DAX will follow
the  complete chain of  relationship and return the related column value.
In a one-to-many relationship, `RELATED` can access the one-side from the many-side. If no such row
exists, `RELATED` simply returns `BLANK`.

```
Sales[AdjustedCost] =
IF(
  RELATED('Product Category'[Category]) = "Cell Phone",
  Sales[UnitCost] * 0.95,
  Sales[UnitCost]
)
```

#### RELATEDTABLE

`RELATEDTABLE` returns a table containing all the rows related to the current one.
</br>
`RELATEDTABLE` follow a chain of relationship always starting from the one-side and going in the 
direction of the many-side.

```
=COUNTROWS(
  RELATEDTABLE(Product)
)
```
