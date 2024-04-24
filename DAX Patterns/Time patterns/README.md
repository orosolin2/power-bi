# DAX Patterns

## Time patterns
https://www.daxpatterns.com/time-patterns/

A time-related calculation refers to any calculation that involves time.</br>

Examples include the set of period-to date calculations, like year-to-date, quarter-to-date, or 
month-to-date.</br>

These calculations accumulate values from the beggining of a time period - year, quarter, month - 
and they return the aggregation of the measure from the start of the period to the date shown in 
the report.</br>

The definition of a time period changes depending on wheter you work with the Gregorian calendar or a fiscal calendar.</br>

Included in these patterns are also comparisons of a parameter over a certain period od time, with a different period of time. For example, you can compare the sales of the current month against the sales of
the same month in the previous year.</br>

Another example of time-related calculations is the moving average over a time period, like a rolling average over 12 months which smoothes out line charts and removes the effect of seasonality from calculations.</br>

What makes the patterns so different from one another, is the definition of what a calendar is. Depending on whether you are working with the Gregorian or the fiscal calendar, the numbers are different.

### 1. Standard time-related calculations
https://www.daxpatterns.com/standard-time-related-calculations/</br>
https://www.daxpatterns.com/standard-time-related-calculations/#video</br>

This pattern is implemented using regular DAX time intelligence functinos.</br>

It  works bases on the assumption that your calendar is a regular Gregorian calendar and that your fiscal calendar starts at the beggining of a Gregorian quarter.</br>

If your are using a regfular Gregorian calendar, then the formulas in this pattern are the easiest and most effective way of producing time intelligence calculations. Keep in mind that standard DAX time intelligence functions only support a regular Gregorian calendar - that is a calendar witrh 12 months, each month with its Gregorian number of days, quarters made up of three months, and all the regular aspects of a calendar that we are used to.


### 2. Month-related calculations
### 3. Week-related calculations
### 4. Custom time-related calculations
