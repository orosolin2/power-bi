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
