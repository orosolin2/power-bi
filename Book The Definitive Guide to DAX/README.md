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
