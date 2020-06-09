#######
Indexes
#######

-----


Interest Rate
#############

IborIndex
*********

.. function:: ql.IborIndex(familyName, tenor, settlementDays, currency, fixingCalendar, convention, endOfMonth, dayCounter, =Handleql.YieldTermStructure())

.. code-block:: python

    ql.IborIndex('MyIndex', ql.Period('6m'), 2, ql.EURCurrency(), ql.TARGET(), ql.ModifiedFollowing, True, ql.Actual360())
    ql.Libor('MyIndex', ql.Period('6M'), 2, ql.USDCurrency(), ql.TARGET(), ql.Actual360())
    ql.Euribor(ql.Period('6M'))        
    ql.USDLibor(ql.Period('6M'))
    ql.Euribor6M()

Derived Classes: 

- ql.Euribor()

.. function:: ql.Euribor(period)

.. function:: ql.Euribor(period, yts)



OvernightIndex
**************

.. function:: ql.OvernightIndex(name, fixingDays, currency, calendar, dayCounter, =ql.YieldTermStructureHandle())


.. code-block:: python

    name = 'CNYRepo7D'
    fixingDays = 1
    currency = ql.CNYCurrency()
    calendar = ql.China()
    dayCounter = ql.Actual365Fixed()
    overnight_index = ql.OvernightIndex(name, fixingDays, currency, calendar, dayCounter)


-----


SwapIndex
*********

.. function:: ql.SwapIndex(familyName, tenor, settlementDays, currency, fixingCalendar, fixedLegTenor, convention, dayCounter, index, =Handleql.YieldTermStructure())


Derived Classes: 

- ql.ChfLiborSwapIsdaFix
- ql.EuriborSwapIsdaFixA
- ql.EuriborSwapIsdaFixB
- ql.EuriborSwapIfrFix
- ql.EurLiborSwapIfrFix
- ql.EurLiborSwapIsdaFixA
- ql.EurLiborSwapIsdaFixB
- ql.GbpLiborSwapIsdaFix
- ql.JpyLiborSwapIsdaFixAm
- ql.JpyLiborSwapIsdaFixPm
- ql.OvernightIndexedSwapIndex
- ql.UsdLiborSwapIsdaFixAm
- ql.UsdLiborSwapIsdaFixPm


Constructors for derived classes:

.. function:: ql.EuriborSwapIsdaFixA(period)

.. function:: ql.EuriborSwapIsdaFixA(period, yts)

.. function:: ql.EuriborSwapIsdaFixA(period, forward_yts, discounting_yts)

-----


SwapSpreadIndex
***************

.. function:: SwapSpreadIndex (familyName, swapIndex1, swapIndex2, gearing1=1.0, gearing2=-1.0)



Inflation
#########

Zero Inflation
**************

.. function:: ql.{InflationIndex}(interpolated=bool)

.. function:: ql.{InflationIndex}(bool, ZeroInflationTermStructure)

- ql.UKRPI
- ql.USCPI
- ql.EUHICP
- ql.EUHICPXT


YoY inflation
*************

- ql.YYEUHICP
- ql.YYEUHICPXT
- ql.YYFRHICP
- ql.YYUKRPI
- ql.YYUSCPI
- ql.YYZACPI


-----

Fixings
#######

.. code-block:: python


    fixingDates = [cf.fixingDate() for cf in map(ql.as_floating_rate_coupon, loan)]
    euribor3m.clearFixings()

    euribor3m.addFixing(ql.Date(17, 7, 2018), -0.3)
    euribor3m.addFixings([ql.Date(12, 7, 2018), ql.Date(13, 7, 2018)], [-0.3, -0.3])    


.. code-block:: python

    [dt for dt in index.timeSeries().dates()]
    [dt for dt in index.timeSeries().values()]


To get the fixing dates form an instrument:

.. code-block:: python

    swap3 = ql.MakeVanillaSwap(ql.Period('3y'), ql.Euribor6M(), 0.01, ql.Period("-2D"))
    fixingDates = [cf.fixingDate() for cf in map(ql.as_floating_rate_coupon, swap3.floatingLeg())]


Indexes have calendars and will not accept invalid fixing dates:

.. code-block:: python

    index.isValidFixingDate(ql.Date(25,12,2019))
    c = index.fixingCalendar()
    c.name()


IndexManager
############

.. code-block:: python

    ql.IndexManager.instance().histories()

    for dt, value in zip(im.getHistory('EURIBOR6M ACTUAL/360').dates(),im.getHistory('EURIBOR6M ACTUAL/360').values()):
        print(dt, value)


    IndexManager.instance().clearHistory(index.name())


