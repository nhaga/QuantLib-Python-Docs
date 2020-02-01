########
Dates
########

------

Date
####

**Constructors**

.. function:: ql.Date(serialNumber)
serialNumber is an integer, such as 24214, and 1 corresponds to 1899-12-31. The usage is the same as in Excel. (The range of serialNumber is limited to 367 to 109574, and the corresponding date range is 1901-01-01 to 2199-12-31.)

.. code-block:: python

    ql.Date(44000)

.. function:: ql.Date(day, month, year)
where day and year are integers; month can be an integer or a special object reserved in quantlib-python that is specifically used to represent the month (ql.January (equal to 1), ..., ql.December(equal to 12))

.. code-block:: python

    ql.Date(18, 6, 2020)
    ql.Date(18, ql.June, 2020)


.. function:: ql.Date(dateString, formatString)

.. code-block:: python

    ql.Date('18-06-2020', '%d-%m-%Y')


**Member functions**

- **ISO**
- **weekday()** : an integer that returns the number corresponding to the week:
  - Sunday: 1
  - ...
  - Saturday: 7
- **dayOfMonth()** : integer, the date returned is the day of the month
- **dayOfYear()** : integer, the date returned is the day of the year
- **month()** : an integer that returns the month corresponding to the date
- **year()** : an integer that returns the year corresponding to the date
- **serialNumber()** integer, the number of days corresponding to the return date (starting from 1899-12-31)

.. code-block:: python

    print('Original Date:', today)
    print('ISO format:', today.ISO())
    print('Weekday:', today.weekday())
    print('Day of Month:', today.dayOfMonth())
    print('Day of Year:', today.dayOfYear())
    print('Month:', today.month())
    print('Year:', today.year())
    print('Serial Number:', today.serialNumber())


**Static functions**

- **Date.todaysDate()**: Date object, which returns the current date of the system.
- **Date.minDate()**: Date object, which returns the minimum date that QuantLib can represent.
- **Date.maxDate()**: Date object, which returns the maximum date that QuantLib can represent.
- **Date.isLeap(y)**: Boolean value to determine whether y is a leap year
- **Date.endOfMonth(d)**: Date object, which returns the date corresponding to the end of the month where the date d is located
- **Date.isEndOfMonth(d)**: Boolean value to determine whether d is at the end of the month
- **Date.nextWeekday(d, w)**: Date object, which returns the date corresponding to the first week w after date d (for example, the first Friday after 2018-03-12)
- **Date.nthWeekday(n, w, m, y)**: Date object, which returns the date corresponding to the n week w in the given month m and year y (for example, the third Wednesday of July 2010)

.. code-block:: python

    print('Today :', ql.Date.todaysDate())
    print('Min Date :', ql.Date.minDate())
    print('Max Date :', ql.Date.maxDate())
    print('Is Leap :', ql.Date.isLeap(2011))
    print('End of Month :', ql.Date.endOfMonth(ql.Date(4, ql.August, 2009)))
    print('Is Month End :', ql.Date.isEndOfMonth(ql.Date(29, ql.September, 2009)))
    print('Is Month End :', ql.Date.isEndOfMonth(ql.Date(30, ql.September, 2009)))
    print('Next WD :', ql.Date.nextWeekday(ql.Date(1, ql.September, 2009), ql.Friday))
    print('n-th WD :', ql.Date.nthWeekday(3, ql.Wednesday, ql.September, 2009))


-----

Period
######

.. function:: ql.Period(n, units)

.. code-block:: python

    ql.Period(1, ql.Days)


.. function:: ql.Period(periodString)

.. code-block:: python

    ql.Period('1d')


.. function:: ql.Period(frequency)

.. code-block:: python

    ql.Period(ql.Annual)



-----

Calendar
########

----



DayCounter
##########

https://www.isda.org/a/pIJEE/The-Actual-Actual-Day-Count-Fraction-1999.pdf

The “Day Count Convention” is critical for the valuation of financial products, especially for fixed-income products. QuantLib provides the following common rules:
- **Actual360**: Actual / 360

- **Actual365Fixed**: Actual / 365(Fixed)
 - Standard
 - Canadian
 - NoLeap
- **ActualActual**: Actual / Actual
 - ISMA
 - Bond
 - ISDA
 - Historical
 - Actual365
 - AFB
 - Euro
- **Business252**: Business / 252
- **Thirty360**: 30 / 360
- **SimpleDayCounter**

.. code-block:: python

    dayCounters = {
        'SimpleDayCounter': ql.SimpleDayCounter(),
        'Thirty360': ql.Thirty360(),
        'Actual360': ql.Actual360(),
        'Actual365Fixed': ql.Actual365Fixed(),
        'Actual365Fixed(Canadian)': ql.Actual365Fixed(ql.Actual365Fixed.Canadian),
        'Actual365NoLeap': ql.Actual365NoLeap(),
        'ActualActual': ql.ActualActual(),
        'Business252': ql.Business252()    
    }


    startDate = ql.Date(15,5,2015)
    endDate = ql.Date(15,6,2015)
    r = 0.05
    nominal = 100e6

    for name, dc in dayCounters.items():
        amount = ql.FixedRateCoupon(endDate, nominal, r, dc, startDate, endDate).amount()
        print(name, f"{amount:,.2f}")


----


Schedule
########

.. function:: Schedule(effectiveDate , terminationDate , tenor, calendar, convention, terminationDateConvention, rule, endOfMonth, firstDate = Date (), nextToLastDate = Date ())
----



DateGeneration
##############

The valuation of many products relies on an analysis of future cash flows, so accurately generating a list of dates for future cash flows is crucial.
After the start and end dates are given, the date list can be generated in the manner of "reverse method" or "forward method".

Example:

Monthly periods with start date is 07-05-2020 and the end date is 15-08-2020:

.. code-block:: python

    start = ql.Date(7,5,2020)
    end = ql.Date(15,8,2020)

    rules = {
        'Backward': ql.DateGeneration.Backward,
        'Forward': ql.DateGeneration.Forward,
        'Zero': ql.DateGeneration.Zero,
        'ThirdWednesDay': ql.DateGeneration.ThirdWednesday,
        'Twentieth': ql.DateGeneration.Twentieth,
        'TwentiethIMM': ql.DateGeneration.TwentiethIMM,
        'CDS': ql.DateGeneration.CDS

    }

    for name, rule in rules.items():
        schedule = ql.MakeSchedule(start, end, ql.Period('1m'), rule=rule)
        print(name, [dt for dt in schedule])



----


TimeGrid
########
