# DAX Patterns

## Time patterns 

### Standard time-related calculations
https://www.daxpatterns.com/standard-time-related-calculations/</br>

This pattern is implemented using regular DAX time intelligence functinos.</br>

It  works bases on the assumption that your calendar is a regular Gregorian calendar and that your fiscal calendar starts at the beggining of a Gregorian quarter.</br>

If your are using a regular Gregorian calendar, then the formulas in this pattern are the easiest and most effective way of producing time intelligence calculations. Keep in mind that standard DAX time intelligence functions only support a regular Gregorian calendar - that is a calendar witrh 12 months, each month with its Gregorian number of days, quarters made up of three months, and all the regular aspects of a calendar that we are used to.</br>

In order to use any time intelligence calculation, you need a well-formed date table. The _Date_ table must satisfy the following requirements:

* All dates need to be present for the years required. The _Date_ table must alwaysstart on January 1 and end on December 31, including all the days in this range. If the report only references fiscal years, then the _Date_ table must include all the dates from the first to the last day of a fiscal year.
* There need to be a column with a _DateTime_ or _Date_ data type containing unique values. This column is usually called _Date_. Even though the _Date_ column is often used to define relationships with another tables, this is not required. Still, the _Date_ column must contain unique values and should be referenced by the Mark as Date Table feature. In case the column also contains a time part, no time should be used - for example, the time should always be 12:00 am.
* The _Date_ table must be marked as a date table in the model, in case the relationship between the _Date_ table and any other table is not based on the _Date_.

There are several ways to build a _Date_ table. The way you build the _Date_ table does not affect how you use the standard time intelligence calculations, as long as the date table satisfies the requirements. If you already have a _Date_ table that works weel for your report, just import it and mark it as a date table after having checked that it satisfies the minimum requirements.</br>

The Mark as Date Table setting adds a `REMOVEFILTERS` modifier over the _Date_ table every time a filter is applied to the _Date_ column. This action is performed by all the time intelligence functions used in `CALCULATE`. DAX implements
