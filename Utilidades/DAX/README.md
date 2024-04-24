# Power BI

## Utilidades

### DAX

#### Tabela Calendário v01

```
Date = 
VAR FirstFiscalMonth = 7  -- First month of fiscal year
VAR FirstDayOfWeek = 0    -- 0 = Sunday, 1 = Monday, ...
VAR FirstYear =           -- Customize first year to use
    YEAR ( MIN ( Sales[Order Date]  ))
RETURN
GENERATE (
    FILTER (
        CALENDARAUTO (),
        YEAR ( [Date] ) >= FirstYear
    ),
    VAR Yr = YEAR ( [Date] )             -- Year Number
    VAR Mn = MONTH ( [Date] )            -- Month Number (1-12)
    VAR Qr = QUARTER ( [Date] )          -- Quarter Number (1-4)
    VAR MnQ = Mn - 3 * (Qr - 1)          -- Month in Quarter (1-3)
    VAR Wd = WEEKDAY ( [Date], 1 ) - 1   -- Week day number (0 = Sunday, 1 = Monday, ...)
    VAR Fyr =                            -- Fiscal Year Number
        Yr + 1 * ( FirstFiscalMonth > 1 && Mn >= FirstFiscalMonth )
    VAR Fqr =                            -- Fiscal Quarter (string)
        FORMAT ( EOMONTH ( [Date], 1 - FirstFiscalMonth ), "\QQ" )
    RETURN ROW (
        "Year", DATE ( Yr, 12, 31 ),
        "Year Quarter", FORMAT ( [Date], "\QQ-YYYY" ),
        "Year Quarter Date", EOMONTH ( [Date], 3 - MnQ ),
        "Quarter", FORMAT ( [Date], "\QQ" ),
        -- "Year Month", EOMONTH ( [Date], 0 ), -- use this for end-of-month
        "Year Month", EOMONTH ( [Date], -1 ) + 1, -- use this for beginning-of-month
        "Month", DATE ( 1900, MONTH ( [Date] ), 1 ),
        "Day of Week", DATE ( 1900, 1, 7 + Wd + (7 * (Wd < FirstDayOfWeek)) ),
        "Fiscal Year", DATE ( Fyr + (FirstFiscalMonth = 1), FirstFiscalMonth, 1 ) - 1,
        "Fiscal Year Quarter", "F" & Fqr & "-" & Fyr,
        "Fiscal Year Quarter Date", EOMONTH ( [Date], 3 - MnQ ),
        "Fiscal Quarter", "F" & Fqr
    )
)
```

#### Tabela Calendário v02

```
dCalendar = 

  GENERATE ( 
    
    CALENDARAUTO(), 
    VAR currentDay = [Date]
    VAR day = DAY( currentDay )
    VAR month =  FORMAT ( currentDay , "MM" )
    VAR monthname = FORMAT ( currentDay , "MMMM" )
    VAR shortmonthname = FORMAT ( currentDay , "MMM" )
    VAR year =  YEAR ( currentDay )
    VAR DayofWeek = FORMAT ( [Date], "dddd" )
    VAR Quarter = "Q" & FORMAT ( [Date], "Q" ) 
    VAR Week = IF(WEEKNUM([Date],2)>=10,"W" & WEEKNUM([Date],2),"W0" & WEEKNUM([Date],2))
    VAR WeekNumber = WEEKNUM([Date],2)
    VAR ISOYear = 
        IF(AND(WEEKNUM([Date],21) < 5 , WEEKNUM([Date],2) > 50) , year+1 ,
            IF(AND(WEEKNUM([Date], 21) > 50 , WEEKNUM([Date], 2) < 5 ), [Date] - 1 , year)
        )
    VAR ISOWeek = WEEKNUM([Date],21) &"."& ISOYear
    VAR YearWeek = year * 100 + WeekNumber
    VAR DateKey = FORMAT([Date],"YYYYMMDD")
    VAR WeekYear = WeekNumber&"."&year
    VAR YearMonthNumber = year * 100 + month
    VAR WeekDay = WEEKDAY([Date],2)
    VAR CalMonthNumber =
            MONTH ( [Date] )
    VAR CalDayOfMonth =
            DAY ( currentDay )
    VAR _CheckLeapYearBefore =
            year -
            IF ( (CalMonthNumber = 2 && CalDayOfMonth < 29)
                     || CalMonthNumber < 2,
                1,
                0 )
    VAR LeapYearsBefore1900 =
        INT ( 1899 / 4 )
        - INT ( 1899 / 100 )
        + INT ( 1899 / 400 )
    VAR LeapYearsBetween =
            INT ( _CheckLeapYearBefore / 4 )
                - INT ( _CheckLeapYearBefore / 100 )
                + INT ( _CheckLeapYearBefore / 400 )
                - LeapYearsBefore1900
    VAR Sequential365DayNumber =
            INT ( currentDay - LeapYearsBetween )
    VAR CurOffSetMonth = DATEDIFF(TODAY(),[Date],MONTH)
  RETURN   ROW ( 
    "day", day, 
    "Month No", month,
    "Month" , monthname,
    "Month MMM",shortmonthname,
    "MMM Year", UPPER(shortmonthname) &" "& year,
    "year", year,
    "year/month", CONCATENATE(CONCATENATE(FORMAT(year,"####")," "),shortmonthname),
    "Month.Year",FORMAT([Date],"MM.yyyy"),
    "DayofWeek",DayofWeek,
    "Quarter",Quarter,
    "Week",Week,
    "ISOWeek",ISOWeek,
    "WeekNumber",WeekNumber,
    "YearWeek",YearWeek,
    "DateKey",DateKey,
    "Week No / Year",WeekYear,
    "YearMonthNumber",YearMonthNumber,
    "Sequential365DayNumber",Sequential365DayNumber,
    "Week Day",WeekDay,
    "CurOffSetMonth",CurOffSetMonth,
    "YTD",  IF(AND(YEAR([Date])=YEAR(TODAY()),[Date]<=TODAY()),TRUE(),FALSE()),
    "YTD-1",IF(AND(YEAR([Date])=YEAR(TODAY())-1,[Date]<=DATE(YEAR(TODAY())-1,MONTH(TODAY()),DAY(TODAY()))),TRUE(),FALSE())
    ))
```
