#####################
Dates and Conventions
#####################


Conventions
###########

Compounding
-----------

- `ql.Simple`
- `ql.Compounded`
- `ql.Continuous`
- `ql.SimpleThenCompounded`
- `ql.CompoundedThenSimple`


Frequencies
-----------

- `ql.NoFrequency` : no interest;
- `ql.Once` : pay interest once, common in zero-coupon bonds;
- `ql.Annual` : paying interest once a year;
- `ql.Semiannual` : Semiannual interest semi-annually;
- `ql.EveryFourthMonth` : every 4 months;
- `ql.Quarterly` : Quarterly quarterly;
- `ql.Bimonthly` : paying interest every two months;
- `ql.Monthly` : monthly interest payment;
- `ql.EveryFourthWeek` : every 4 weeks;
- `ql.Biweekly` : Biweekly interest every two weeks;
- `ql.Weekly` : paying once a week;
- `ql.Daily` : pay interest once a day.

Weekday correction
------------------


- `ql.Following` : The date is corrected to the first working day that follows.
- `ql.ModifiedFollowing` : The date is corrected to the first working day after that, unless this working day is in the next month; if the modified working day is in the next month, the date is corrected to the last working day that appears before, to ensure the original The date and the revised date are in the same month.
- `ql.Preceding` : Correct the date to the last business day that Preceding before.
- `ql.ModifiedPreceding` : modify the date to the last working day that appeared before, unless the working sunrise is now the previous month; if the modified working sunrise is now the previous month, the date is modified to the first working day after that The original date and the revised date are guaranteed to be in the same month.
- `ql.Unadjusted` : No adjustment.

DateGeneration
--------------

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

- **Date.todaysDate()** : Date object, which returns the current date of the system.
- **Date.minDate()** : Date object, which returns the minimum date that QuantLib can represent.
- **Date.maxDate()** : Date object, which returns the maximum date that QuantLib can represent.
- **Date.isLeap(y)** : Boolean value to determine whether y is a leap year
- **Date.endOfMonth(d)** : Date object, which returns the date corresponding to the end of the month where the date d is located
- **Date.isEndOfMonth(d)** : Boolean value to determine whether d is at the end of the month
- **Date.nextWeekday(d, w)** : Date object, which returns the date corresponding to the first week w after date d (for example, the first Friday after 2018-03-12)
- **Date.nthWeekday(n, w, m, y)** : Date object, which returns the date corresponding to the n week w in the given month m and year y (for example, the third Wednesday of July 2010)

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

The class `ql.Calendar` provides the interface for determining whether a date is a business day or a holiday for a given exchange or a given country, and for incrementing/decrementing a date of a given number of business days.

**Available Calendars**

Argentina, Australia, Austria, BespokeCalendar, Botswana, Brazil, Canada, China, CzechRepublic, Denmark, Finland, France, Germany, HongKong, Hungary, Iceland, India, Indonesia, Israel, Italy, Japan, JointCalendar, Mexico, NewZealand, Norway, NullCalendar, Poland, Romania, Russia, SaudiArabia, Singapore, Slovakia, SouthAfrica, SouthKorea, Sweden, Switzerland, Taiwan, TARGET, Thailand, Turkey, Ukraine, UnitedKingdom, UnitedStates, WeekendsOnly


.. code-block:: python

    calendar1 = ql.UnitedKingdom()
    calendar2 = ql.TARGET()


**Calendar Markets**

| Argentina : ['Merval']
| Brazil : ['Exchange', 'Settlement']
| Canada : ['Settlement', 'TSX']
| China : ['IB', 'SSE']
| CzechRepublic : ['PSE']
| France : ['Exchange', 'Settlement']
| Germany : ['Eurex', 'FrankfurtStockExchange', 'Settlement', 'Xetra']
| HongKong : ['HKEx']
| Iceland : ['ICEX']
| India : ['NSE']
| Indonesia : ['BEJ', 'JSX']
| Israel : ['Settlement', 'TASE']
| Italy : ['Exchange', 'Settlement']
| Mexico : ['BMV']
| Russia : ['MOEX', 'Settlement']
| SaudiArabia : ['Tadawul']
| Singapore : ['SGX']
| Slovakia : ['BSSE']
| SouthKorea : ['KRX', 'Settlement']
| Taiwan : ['TSEC']
| Ukraine : ['USE']
| UnitedKingdom : ['Exchange', 'Metals', 'Settlement']
| UnitedStates : ['FederalReserve', 'GovernmentBond', 'LiborImpact', 'NERC', 'NYSE', 'Settlement']


.. code-block:: python

    calendar1 = ql.UnitedKingdom(ql.UnitedKingdom.Metals)
    calendar2 = ql.UnitedStates(ql.UnitedStates.NYSE)




Some commonly used member functions:

- **isBusinessDay(d)** : A Boolean value that determines whether d is a business day.
- **isHoliday(d)** : A boolean value that determines whether d is a holiday.
- **isWeekend(w)** : A Boolean value that determines whether w is a weekend (in some countries, weekends are not scheduled on Saturdays and Sundays).
- **isEndOfMonth(d)** : A boolean value that determines whether d is the last working day at the end of the month.
- **endOfMonth(d)** : date, returns the last working day of the month in which d is located.

.. code-block:: python

    cal = ql.TARGET()
    mydate = ql.Date(1, ql.May, 2017)

    print('Is BD :', cal.isBusinessDay(mydate))
    print('Is Holiday :', cal.isHoliday(mydate))
    print('Is Weekend :', cal.isWeekend(ql.Friday))
    print('Is Last BD :', cal.isEndOfMonth(ql.Date(5, ql.April, 2018)))
    print('Last BD :', cal.endOfMonth(mydate))


**Custom Holiday List**

The Calendar object in QuantLib can conveniently implement custom holidays. Generally, only the following two functions are needed:

- **addHoliday(d)** : add d as a holiday.
- **removeHoliday(d)** : remove d from the holiday table.


.. code-block:: python


    cal = ql.TARGET()

    day1 = ql.Date(26, 2, 2020)
    day2 = ql.Date(10, 4, 2020)

    print('Is Business Day : ', cal.isBusinessDay(day1))
    print('Is Business Day : ', cal.isBusinessDay(day2))

    cal.addHoliday(day1)
    cal.removeHoliday(day2)

    print('Is Business Day : ', cal.isBusinessDay(day1))
    print('Is Business Day : ', cal.isBusinessDay(day2))




.. code-block:: python

    myCalendar = ql.WeekendsOnly()
    days = [1,14,15,1,21,26,2,16,15,18,19,9,27,1,19,8,17,25,31]
    months = [1,4,4,5,5,6,8,9,9,10,10,11,12,12,12,12]
    name = ['Año Nuevo','Viernes Santo','Sabado Santo','Dia del Trabajo','Dia de las Glorias Navales','San Pedro y San Pablo','Elecciones Primarias','Dia de la Virgen del Carmen','Asuncion de la Virgen','Independencia Nacional','Glorias del Ejercito','Encuentro de dos mundos','Día de las Iglesias Evangélicas y Protestantes','Día de todos los Santos','Elecciones Presidenciales y Parlamentarias','Inmaculada Concepción','Segunda vuelta Presidenciales','Navidad','Feriado Bancario']
    start_year = 2018
    n_years = 10
    for i in range(n_years+1):
        for x,y in zip(days,months):
            date = ql.Date(x,y,start_year+i)
            myCalendar.addHoliday(date)



**Holiday List**

Returns the holidays between two dates.

.. function:: ql.Calendar.holidayList (calendar, from, to, includeWeekEnds=False)

.. code-block:: python

    ql.Calendar.holidayList(ql.TARGET(), ql.Date(1,12,2019), ql.Date(31,12,2019))


Calendar object uses the following two functions to modify the date:

- **adjust(d, convention)** : Date, modify d according to the convention conversion mode.
- **advance(d, period, convention, endOfMonth)** : date, the date is moved backward by time interval period and then modified according to the conversion mode convention ; the parameter endOfMonth indicates that if d is the end of the month, the date after the correction is also at the end of the month.

Finally, the following function can be used to calculate the number of working days during the two days:

- **businessDaysBetween(from, to, includeFirst, includeLast)** : Calculate the number of working days between the dates from and to (whether or not the dates are included).



.. code-block:: python

    cal = ql.TARGET()

    firstDate = ql.Date(31, ql.January, 2018)
    secondDate = ql.Date(1, ql.April, 2018)

    print('Date 2 Adj :', cal.adjust(secondDate, ql.Preceding))
    print('Date 2 Adj :', cal.adjust(secondDate, ql.ModifiedPreceding))

    mat = ql.Period(2, ql.Months)

    print('Date 1 Month Adv :',
        cal.advance(firstDate, mat, ql.Following, False))
    print('Date 1 Month Adv :',
        cal.advance(firstDate, mat, ql.ModifiedFollowing, False))

    print('Business Days Between :',
        cal.businessDaysBetween(
            ql.Date(5, ql.March, 2018), ql.Date(30, ql.March, 2018),
            True, True))



**JointCalenar**

.. function:: ql.JointCalendar(calendar1, calendar2, calendar3, calendar4, JointCalendarRule=JoinHolidays)

.. code-block:: python

    joint_calendar = ql.JointCalendar(ql.TARGET(), ql.Poland())

----



DayCounter
##########

https://www.isda.org/a/pIJEE/The-Actual-Actual-Day-Count-Fraction-1999.pdf

The “Day Count Convention” is critical for the valuation of financial products, especially for fixed-income products. QuantLib provides the following common rules:

- **Actual360** : Actual / 360

- **Actual365Fixed** : Actual / 365(Fixed)
 - Standard
 - Canadian
 - NoLeap
- **ActualActual** : Actual / Actual
 - ISMA
 - Bond
 - ISDA
 - Historical
 - Actual365
 - AFB
 - Euro
- **Business252** : Business / 252
- **Thirty360** : 30 / 360
- **SimpleDayCounter**

.. code-block:: python

    dayCounters = {
        'SimpleDayCounter': ql.SimpleDayCounter(),
        'Thirty360': ql.Thirty360(),
        'Actual360': ql.Actual360(),
        'Actual365Fixed': ql.Actual365Fixed(),
        'Actual365Fixed(Canadian)': ql.Actual365Fixed(ql.Actual365Fixed.Canadian),
        'Actual365FixedNoLeap': ql.Actual365Fixed(ql.Actual365Fixed.NoLeap),
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


The types and explanations of these variables are as follows:

- **effectiveDate**, **terminationDate** : Date, the start and end date of the calendar list, such as the value date and expiration date of the bond.
- **tenor** : Period object, the interval between two adjacent dates, such as the bond frequency (1 year or 6 months) or interest rate swap rate (3 months).
- **calendar** : A calendar table that generates a specific calendar of dates to follow.
- **convention** : integer, how to adjust the non-working day (except the last date), the value range is some reserved variables of quantlib-python.
- **terminationDateConvention** : Integer, if the last date is a non-working day, how to adjust it, the value range is some reserved variables of quantlib-python.
- **Rule** : A member of DateGeneration that generates the rules for the date.
- **endOfMonth** : If the start date is at the end of the month, whether other dates are required to be scheduled at the end of the month (except the last date).
- **firstDate** : nextToLastDate (optional): Date, the start and end date (not commonly used) provided for the generated method rule .


.. code-block:: python

    effectiveDate = ql.Date(15,6,2020)
    terminationDate = ql.Date(15,6,2022)
    frequency = ql.Period('6M')
    calendar = ql.TARGET()
    convention = ql.ModifiedFollowing
    terminationDateConvention = ql.ModifiedFollowing
    rule = ql.DateGeneration.Backward
    endOfMonth = False
    schedule = ql.Schedule(effectiveDate, terminationDate, frequency, calendar, convention, terminationDateConvention, rule, endOfMonth)


MakeSchedule
############

.. function:: ql.MakeSchedule(effectiveDate, terminationDate, frequency)

Optional params:

- calendar=None
- convention=None
- terminalDateConvention=None,
- rule=None
- forwards=False
- backwards=False,
- endOfMonth=None
- firstDate=None
- nextToLastDate=None

.. code-block:: python

    effectiveDate = ql.Date(15,6,2020)
    terminationDate = ql.Date(15,6,2022)
    frequency = ql.Period('6M')
    schedule = ql.MakeSchedule(effectiveDate, terminationDate, frequency)


----


TimeGrid
########

.. function:: ql.TimeGrid(end, steps)

.. code-block:: python

    t = ql.TimeGrid(10, 5)
    t.dt(4)


If there are certain times that need to appear in the TimeGrid, pass them in as a list

.. function:: ql.TimeGrid(requiredTimes, steps)

.. code-block:: python

    [t for t in ql.TimeGrid([1, 2.5, 4], 10)]
