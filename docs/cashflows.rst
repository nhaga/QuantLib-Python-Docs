##################################
CashFlows, Legs and Interest Rates
##################################

------


Interest Rates
##############

Concrete interest rate class

.. function:: ql.InterestRate(r, dc, comp, freq)

------


SimpleCashFlow
##############

.. function:: ql.SimpleCashFlow (amount, date)

    Update the global evaluation date


.. code-block:: python

    amount = 105
    date = ql.Date(15,6,2020)
    cf = ql.SimpleCashFlow(amount, date)


**Functions**

.. function:: .amount()

.. function:: .date()


------

Coupons
#######



FixedRateCoupon
***************

.. function:: ql.FixedRateCoupon(paymentDate, nominal, rate, dayCounter, startDate, endDate)

.. code-block:: python

    amount = 105
    nominal = 100.
    paymentDate = ql.Date(15,6,2020)
    startDate = ql.Date(15,12,2019)
    rate = .05
    dayCounter = ql.Actual360()
    coupon = ql.FixedRateCoupon(endDate, nominal, rate, dayCounter, startDate, endDate)


IborCoupon
**********

.. function:: ql.IborCoupon(paymentDate, nominal, startDate, endDate, fixingDays, index)

.. code-block:: python

    nominal = 100.
    startDate = ql.Date(15,12,2020)
    endDate = ql.Date(15,6,2021)
    rate = .05
    dayCounter = ql.Actual360()
    index = ql.Euribor6M()
    coupon = ql.IborCoupon(endDate, nominal, startDate, endDate, 2, index)


CappedFlooredCoupon
*******************

Capped and/or floored floating-rate coupon

.. function:: ql.CappedFlooredCoupon(FloatingRateCoupon, cap, floor)


CappedFlooredIborCoupon
***********************



CmsCoupon
*********

.. function:: ql.CmsCoupon(endDate, nominal, startDate, endDate, fixingDays, swapIndex)

.. code-block:: python

    nominal = 100.
    startDate = ql.Date(15,12,2020)
    endDate = ql.Date(15,6,2021)
    rate = .05
    dayCounter = ql.Actual360()
    index = ql.Euribor6M()
    fixingDays = 2
    swapIndex = ql.EuriborSwapIsdaFixA(ql.Period("2Y"))
    cms = ql.CmsCoupon(endDate, nominal, startDate, endDate, fixingDays, swapIndex)

CappedFlooredCmsCoupon
**********************

.. function:: ql.CappedFlooredCmsCoupon(endDate, nominal, startDate, endDate, fixingDays, swapIndex, rate, spread)


CmsSpreadCoupon
***************

.. function:: ql.CmsSpreadCoupon(endDate, nominal, startDate, endDate, fixingDays, spreadIndex)

.. function:: ql.CmsSpreadCoupon(endDate, nominal, startDate, endDate, fixingDays, spreadIndex, gearing=1, spread=0, refPeriodStart=ql.Date(), refPeriodEnd=ql.Date(), dayCounter=ql.DayCounter(), isInArrears=False, exCouponDate=ql.Date())

.. code-block:: python

    nominal = 100.
    startDate = ql.Date(15,12,2020)
    endDate = ql.Date(15,6,2021)
    rate = .05
    dayCounter = ql.Actual360()
    index = ql.Euribor6M()
    fixingDays = 2
    swapIndex1 = ql.EuriborSwapIsdaFixA(ql.Period("10Y"))
    swapIndex2 = ql.EuriborSwapIsdaFixA(ql.Period("2Y"))
    spreadIndex = ql.SwapSpreadIndex("CMS 10Y-2Y", swapIndex1, swapIndex2)
    spread = ql.CmsSpreadCoupon(endDate, nominal, startDate, endDate, fixingDays, spreadIndex)






------

Legs
####

Leg
***

.. code-block:: python

    date = ql.Date().todaysDate()
    cf1 = ql.SimpleCashFlow(5.0, date+365)
    cf2 = ql.SimpleCashFlow(5.0, date+365*2)
    cf3 = ql.SimpleCashFlow(105.0, date+365*3)
    leg = ql.Leg([cf1, cf2, cf3])

FixedRateLeg
************

helper class building a sequence of fixed rate coupons

.. function:: ql.FixedRateLeg(schedule, dayCount, nominals, fixedRate, BusinessDayConvention, FirstPeriodDayCounter, ExCouponPeriod, PaymentCalendar)

.. code-block:: python

    schedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(15,6,2021), ql.Period('6M'))
    dayCount = ql.Actual360()
    leg = ql.FixedRateLeg(schedule, dayCount, [100.], [0.05])
    leg = ql.FixedRateLeg(schedule, ql.Actual360(), [100.], [0.05], ql.Following, ql.Actual360(), ql.Period('3M'), ql.TARGET())

IborLeg
*******

helper class building a sequence of capped/floored ibor-rate coupon

.. function:: ql.IborLeg(nominals, schedule, index, paymentDayCounter = DayCounter(), paymentConvention = Following, fixingDays = 0, gearings = 1, spreads, caps, floors, isInArrears, exCouponPeriod, exCouponCalendar, exCouponConvention = Unadjusted, exCouponEndOfMonth = False)

.. code-block:: python

    schedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(15,6,2021), ql.Period('6M'))
    index = ql.Euribor3M()
    leg = ql.IborLeg([100], schedule, index)


OvernightLeg
************

helper class building a sequence of overnight coupons

.. function:: ql.OvernightLeg(nominals, schedule, overnightIndex, dayCount, BusinessDayConvention, gearing, spread, TelescopicValueDates)

.. code-block:: python

    nominal = 100
    schedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(15,6,2021), ql.Period('3M'))
    overnightIndex = ql.OvernightIndex('CNYRepo7D', 1, ql.CNYCurrency(), ql.China(), ql.Actual365Fixed())
    ql.OvernightLeg([nominal], schedule, overnightIndex, ql.Actual360(), ql.Following, [1],[0], True)



---------


Pricers
#######

BlackIborCouponPricer
*********************

LinearTsrPricer
***************

LognormalCmsSpreadPricer
************************

NumericHaganPricer
******************

AnalyticHaganPricer
*******************


---------


Cashflow Analysis Functions
###########################


- 'atmRate',
- 'basisPointValue',
- 'bps',
- 'convexity',
- 'duration',
- 'maturityDate',
- 'nextCashFlowDate',
- 'npv',
- 'previousCashFlowDate',
- 'startDate',
- 'yieldRate',
- 'zSpread'










