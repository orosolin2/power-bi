# The Definitive Guide to DAX

## Chapter 2: Introducing DAX

### Calculated columns

<p>
  A calculated column is just like any other column in a table and you can use it in rows, columns, 
  filters, or values of a pivot table or any other report. You can also a calculated column to define a 
  relationship, if needed. The DAX expression defined for a calculated column operates in the context of 
  the current row of the table to which it belongs. Any reference to a column returns the value of that
  column for the current row. You canot directly access the values of other rows.
</p>

<p>
  Calculated columns are computed during the database processing and then stored in the model. The time
  required to compute them is always process time and not query time, resulting in a better user experience.
  Nevertheless, you always must remember that calculated column uses precious RAM.
</p>

### Measures

<p>
  A measure is a calculation that aggregate values from many rows in a table. A measure operates on 
  aggregations of data defined by the current context.
</p>

### Variables

<p>
  To avoid repeating the same expression. They are local to the expression in which you define them.
  Variables are computed using lazy evaluation
</p>

```
VAR
  TotalSales = SUM(Sales[SalesAmount])
RETURN
  (TotalSales - SUM(Sales[TotalProductCost])) / TotalSales
```
