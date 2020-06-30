##################################
CashFlows, Legs and Interest Rates
##################################

------


Interest Rates
##############

Concrete interest rate class

.. function:: ql.InterestRate(rate, dayCount, compounding, frequency)

.. code-block:: python

    rate = ql.InterestRate(0.05, ql.Actual360(), ql.Compounded, ql.Annual)


**Compounding**

- Simple
- Compounded
- Continuous

**Frequencies**

- NoFrequency , no interest;
- Once , pay interest once, common in zero-coupon bonds;
- Annual , paying interest once a year;
- Semiannual , Semiannual interest semi-annually;
- EveryFourthMonth every 4 months;
- Quarterly , Quarterly quarterly;
- Bimonthly , paying interest every two months;
- Monthly , monthly interest payment;
- EveryFourthWeek every 4 weeks;
- Biweekly , Biweekly interest every two weeks;
- Weekly , paying once a week;
- Daily , pay interest once a day.

Here are some common member functions:

- **rate()**: a floating point number that returns the value of the rate of return
- **dayCounter()**: DayCounter object, which returns the member variable that controls the day calculation rule;
- **compounding()**: an integer that returns the interest rate method;
- **frequency()**: Integer, returns the frequency of interest payments.
- **discountFactor(d1, d2)**: float, d1 and d2 are both Date objects ( d1 < d2 ), returning the discount factor size from d1 to d2 ;
- **compoundFactor(d1, d2)**: float, d1 and d2 are both Date objects ( d1 < d2 ), returning the size of the interest factor from d1 to d2 ;
- **equivalentRate(resultDC, comp, freq, d1, d2)**: The InterestRate object returns an InterestRate object equivalent to the current object. The configuration parameters of the object include resultDC , comp , freq :
  - Both d1 and d2 are Date objects ( d1 < d2 )
  - **resultDC**, DayCounter object, configure the number of days calculation rules;
  - **comp**, integer, configuration interest rate, the value range is some reserved variables of quantlib-python;
  - **Freq**, integer, configuration payoff frequency, the range of values ​​is some reserved variables of quantlib-python.

In some cases, it is necessary to recalculate the rate of return based on the size of the interest factor. The InterestRate class provides the function impliedRate implement this function:

- **impliedRate(compound, resultDC, comp, freq, d1, d2)**: The InterestRate object returns the inverse calculated InterestRate object whose configuration parameters include resultDC , comp , freq :
  - Both d1 and d2 are Date objects ( d1 < d2 )
  - **resultDC**, DayCounter object, configure the number of days calculation rules;
  - **comp**, integer, configuration interest rate, the value range is some reserved variables of quantlib-python;
  - **Freq**, integer, configuration payoff frequency, the range of values ​​is some reserved variables of quantlib-python.

.. code-block:: python

    print("Rate: ", rate.rate())
    print("DayCount: ", rate.dayCounter())
    print("DiscountFactor: ", rate.discountFactor(1))
    print("DiscountFactor: ", rate.discountFactor(ql.Date(15,6,2020), ql.Date(15,6,2021)))
    print("CompoundFactor: ", rate.compoundFactor(ql.Date(15,6,2020), ql.Date(15,6,2021)))
    print("EquivalentRate: ", rate.equivalentRate(ql.Actual360(), ql.Compounded, ql.Semiannual, ql.Date(15,6,2020), ql.Date(15,6,2021)))

    factor = rate.compoundFactor(ql.Date(15,6,2020), ql.Date(15,6,2021))
    print("ImpliedRate: ", rate.impliedRate(factor, ql.Actual360(), ql.Continuous, ql.Annual, ql.Date(15,6,2020), ql.Date(15,6,2021)))

------


CashFlows
#########

SimpleCashFlow
**************


.. function:: ql.SimpleCashFlow (amount, date)


.. code-block:: python

    amount = 105
    date = ql.Date(15,6,2020)
    cf = ql.SimpleCashFlow(amount, date)


Redemption
**********

.. function:: ql.Redemption(amount, date)

.. code-block:: python

    amount = 100
    date = ql.Date(15,6,2020)
    redemption = ql.Redemption(amount, date)


AmortizingPaymnent
******************

.. function:: ql.AmortizingPayment(amount, date)

.. code-block:: python

    amount = 100
    date = ql.Date(15,6,2020)
    ql.AmortizingPayment(amount, date)


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


OvernightIndexedCoupon
**********************

.. function:: ql.OvernightIndexedCoupon(paymentDate, nominal, startDate, endDate, overnightIndex, gearing=1.0, spread=0.0, refPeriodStart=ql.Date(), refPeriodEnd=ql.Date(), dayCounter=ql.DayCounter(), telescopicValueDates=False)

.. code-block:: python

    paymentDate = ql.Date(15, 9, 2020)
    nominal = 100
    startDate = ql.Date(15, 6, 2002)
    endDate = ql.Date(15,9,2020)
    overnightIndex = ql.Eonia()
    ql.OvernightIndexedCoupon(paymentDate, nominal, startDate, endDate, overnightIndex)


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
    fixingDays = 2
    swapIndex1 = ql.EuriborSwapIsdaFixA(ql.Period("10Y"))
    swapIndex2 = ql.EuriborSwapIsdaFixA(ql.Period("2Y"))
    spreadIndex = ql.SwapSpreadIndex("CMS 10Y-2Y", swapIndex1, swapIndex2)
    ql.CmsSpreadCoupon(endDate, nominal, startDate, endDate, fixingDays, spreadIndex)

CappedFlooredCmsSpreadCoupon
****************************

.. function:: ql.CmsSpreadCoupon(endDate, nominal, startDate, endDate, fixingDays, spreadIndex, gearing=1, spread=0, cap=Null, floor=Null, refPeriodStart=ql.Date(), refPeriodEnd=ql.Date(), dayCounter=ql.DayCounter(), isInArrears=False, exCouponDate=ql.Date())

.. code-block:: python

    nominal = 100.
    startDate = ql.Date(15,12,2020)
    endDate = ql.Date(15,6,2021)
    fixingDays = 2
    swapIndex1 = ql.EuriborSwapIsdaFixA(ql.Period("10Y"))
    swapIndex2 = ql.EuriborSwapIsdaFixA(ql.Period("2Y"))
    spreadIndex = ql.SwapSpreadIndex("CMS 10Y-2Y", swapIndex1, swapIndex2)
    ql.CappedFlooredCmsSpreadCoupon(endDate, nominal, startDate, endDate, fixingDays, spreadIndex)

    gearing = 1
    spread = 0
    cap=0
    floor=0

    ql.CappedFlooredCmsSpreadCoupon(endDate, nominal, startDate, endDate, fixingDays, spreadIndex, gearing, spread, cap, floor)

    refPeriodStart = ql.Date()
    refPeriodEnd = ql.Date()
    dayCounter = ql.Actual360()
    isInArrears = False
    exCouponDate = ql.Date()
    ql.CappedFlooredCmsSpreadCoupon(endDate, nominal, startDate, endDate, fixingDays, spreadIndex, gearing, spread, cap, floor, refPeriodStart, refPeriodEnd, dayCounter, isInArrears, exCouponDate)



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

.. code-block:: python

    leg = ql.IborLeg([100], schedule, index, ql.Actual360())
    leg = ql.IborLeg([100], schedule, index, ql.Actual360(), ql.ModifiedFollowing)
    leg = ql.IborLeg([100], schedule, index, ql.Actual360(), ql.ModifiedFollowing, [2])
    leg = ql.IborLeg([100], schedule, index, ql.Actual360(), ql.ModifiedFollowing, fixingDays=[2], gearings=[1])

    leg = ql.IborLeg([100], schedule, index, ql.Actual360(), ql.ModifiedFollowing, fixingDays=[2], gearings=[1], spreads=[0])
    leg = ql.IborLeg([100], schedule, index, ql.Actual360(), ql.ModifiedFollowing, fixingDays=[2], gearings=[1], spreads=[0], caps=[0])
    leg = ql.IborLeg([100], schedule, index, ql.Actual360(), ql.ModifiedFollowing, fixingDays=[2], gearings=[1], spreads=[0], floors=[0])


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

.. function:: ql.BlackIborCouponPricer(OptionletVolatilityStructureHandle)

.. code-block:: python

    volatility = 0.10;
    vol = ql.ConstantOptionletVolatility(2, ql.TARGET(), ql.Following, volatility, ql.Actual360())
    pricer = ql.BlackIborCouponPricer(ql.OptionletVolatilityStructureHandle(vol))

**Example:** In arrears coupon

.. code-block:: python

    crv = ql.FlatForward(0, ql.TARGET(), -0.01, ql.Actual360())
    yts = ql.YieldTermStructureHandle(crv)
    index = ql.Euribor3M(yts)

    schedule = ql.MakeSchedule(ql.Date(15,6,2021), ql.Date(15,6,2023), ql.Period('6M'))

    leg = ql.IborLeg([100], schedule, index, ql.Actual360(), ql.ModifiedFollowing, isInArrears=True)
        
    volatility = 0.10;
    vol = ql.ConstantOptionletVolatility(2, ql.TARGET(), ql.Following, volatility, ql.Actual360())
    pricer = ql.BlackIborCouponPricer(ql.OptionletVolatilityStructureHandle(vol))
    ql.setCouponPricer(leg, pricer)

    npv = ql.CashFlows.npv(leg, yts, True)    
    print(f"LEG NPV: {npv:,.2f}")


LinearTsrPricer
***************

.. function:: ql.LinearTsrPricer(swaptionVolatilityStructure, meanReversion)

.. code-block:: python

    volQuote = ql.QuoteHandle(ql.SimpleQuote(0.2))
    swaptionVol = ql.ConstantSwaptionVolatility(0, ql.TARGET(), ql.ModifiedFollowing, volQuote, ql.Actual365Fixed())
    swvol_handle = ql.SwaptionVolatilityStructureHandle(swaptionVol)

    mean_reversion = ql.QuoteHandle(ql.SimpleQuote(0.01))
    cms_pricer = ql.LinearTsrPricer(swvol_handle, mean_reversion)

LognormalCmsSpreadPricer
************************

NumericHaganPricer
******************

AnalyticHaganPricer
*******************


---------


Cashflow Analysis Functions
###########################


Date Inspectors
***************

.. function:: ql.CashFlows.startDate(leg)

.. function:: ql.CashFlows.maturityDate(leg)

Cashflow Inspectors
*******************

the last cashflow paying before or at the given date

.. function:: ql.CashFlows.previousCashFlowDate(leg, includeSettlementDateFlows, settlementDate=ql.Date())

.. code-block:: python

    ql.CashFlows.previousCashFlowDate(leg, True)
    ql.CashFlows.previousCashFlowDate(leg, True, ql.Date(15,12,2020))

the first cashflow paying after the given date

.. function:: ql.CashFlows.nextCashFlowDate(leg, includeSettlementDateFlows, settlementDate=ql.Date())


YieldTermstructure
******************

NPV of the cash flows

.. function:: ql.CashFlows.npv(leg, discountCurve, includeSettlementDateFlows, settlementDate=ql.Date(), npvDate=ql.Date())

.. code-block:: python

    yts = ql.YieldTermStructureHandle(ql.FlatForward(ql.Date(15,1,2020), 0.04, ql.Actual360()))
    ql.CashFlows.npv(leg, yts, True)
    ql.CashFlows.npv(leg, yts, True, ql.Date(15,6,2020))
    ql.CashFlows.npv(leg, yts, True, ql.Date(15,6,2020), ql.Date(15,12,2020))


Basis-point sensitivity of the cash flows

.. function:: ql.CashFlows.bps(leg, discountCurve, includeSettlementDateFlows, settlementDate=ql.Date(), npvDate=ql.Date())

.. code-block:: python

    yts = ql.YieldTermStructureHandle(ql.FlatForward(ql.Date(15,1,2020), 0.04, ql.Actual360()))
    ql.CashFlows.bps(leg, yts, True)


At-the-money rate of the cash flows

.. function:: ql.CashFlows.atmRate(leg, discountCurve, includeSettlementDateFlows, settlementDate=ql.Date(), ql.npvDate=Date(), npv=Null< Real >())

.. code-block:: python

    crv = ql.FlatForward(ql.Date(15,1,2020), 0.04, ql.Actual360())
    ql.CashFlows.atmRate(leg, crv, True, ql.Date(15,6,2020))


Yield (a.k.a. Internal Rate of Return, i.e. IRR)
************************************************

.. function:: ql.CashFlows.npv(leg, rate, includeSettlementDateFlows, settlementDate=ql.Date(), npvDate=ql.Date())

.. code-block:: python

    rate = ql.InterestRate(.03, ql.ActualActual(), ql.Compounded, ql.Annual)
    ql.CashFlows.npv(leg, rate, True)


.. function:: ql.CashFlows.bps(leg, rate, includeSettlementDateFlows, settlementDate=ql.Date(), npvDate=ql.Date())

.. code-block:: python

    rate = ql.InterestRate(.03, ql.ActualActual(), ql.Compounded, ql.Annual)
    ql.CashFlows.bps(leg, rate, True)


.. function:: ql.CashFlows.basisPointValue(leg, InterestRate, includeSettlementDateFlows, settlementDate=ql.Date(), ql.npvDate=Date())

.. code-block:: python

    rate = ql.InterestRate(.03, ql.ActualActual(), ql.Compounded, ql.Annual)
    ql.CashFlows.basisPointValue(leg, rate, True)

.. function:: ql.CashFlows.basisPointValue(leg, rate, dayCounter, compounding, frequency, includeSettlementDateFlows,, settlementDate=ql.Date(), ql.npvDate=Date())

.. code-block:: python

    ql.CashFlows.basisPointValue(leg, 0.05, ql.Actual360(), ql.Compounded, ql.Annual, True)


.. function:: ql.CashFlows.duration(leg, InterestRate, ql.Duration.Type, includeSettlementDateFlows, settlementDate=ql.Date(), npvDate=ql.Date())

.. code-block:: python

    rate = ql.InterestRate(.03, ql.ActualActual(), ql.Compounded, ql.Annual)

    ql.CashFlows.duration(leg, rate, ql.Duration.Simple, False)
    ql.CashFlows.duration(leg, rate, ql.Duration.Macaulay, False)
    ql.CashFlows.duration(leg, rate, ql.Duration.Modified, False)

.. function:: ql.CashFlows.duration (leg, rate, dayCounter, compounding, frequency, ql.Duration.Type, includeSettlementDateFlows, settlementDate=ql.Date(), npvDate=ql.Date())

.. code-block:: python

    rate = 0.05
    ql.CashFlows.duration(leg, rate, ql.Actual360(), ql.Compounded, ql.Annual, ql.Duration.Simple, False)

.. function:: ql.CashFlows.convexity(leg, InterestRate, includeSettlementDateFlows, settlementDate=ql.Date(), npvDate=ql.Date())

.. code-block:: python

    rate = ql.InterestRate(.03, ql.ActualActual(), ql.Compounded, ql.Annual)
    ql.CashFlows.convexity(leg, rate, False)

.. function:: ql.CashFlows.convexity(leg, rate, dayCounter, compounding, frequency, includeSettlementDateFlows, settlementDate=ql.Date(), npvDate=ql.Date())

.. code-block:: python

    rate = 0.05
    ql.CashFlows.convexity(leg, rate, ql.Actual360(), ql.Compounded, ql.Annual, False)


.. function:: ql.CashFlows.yieldRate(leg, rate, dayCounter, compounding, frequency, includeSettlementDateFlows, settlementDate=ql.Date(), npvDate=ql.Date(), accuracy=1.0e-10, maxIterations=100, guess=0.0)

.. code-block:: python

    ql.CashFlows.yieldRate(leg, 5, ql.Actual360(), ql.Compounded, ql.Annual, True)
    ql.CashFlows.yieldRate(leg, 5, ql.Actual360(), ql.Compounded, ql.Annual, True, ql.Date(15,6,2020))
    ql.CashFlows.yieldRate(leg, 5, ql.Actual360(), ql.Compounded, ql.Annual, True, ql.Date(15,6,2020), ql.Date(15,12,2020))
    ql.CashFlows.yieldRate(leg, 5, ql.Actual360(), ql.Compounded, ql.Annual, True, ql.Date(15,6,2020), ql.Date(15,12,2020), 1e-5)
    ql.CashFlows.yieldRate(leg, 5, ql.Actual360(), ql.Compounded, ql.Annual, True, ql.Date(15,6,2020), ql.Date(15,12,2020), 1e-5, 100)
    ql.CashFlows.yieldRate(leg, 5, ql.Actual360(), ql.Compounded, ql.Annual, True, ql.Date(15,6,2020), ql.Date(15,12,2020), 1e-5, 100, 0.04)


Z-spread
********

implied Z-spread.

.. function:: ql.CashFlows.zSpread (leg, npv, YieldTermStructure, dayCounter, compounding, frequency, includeSettlementDateFlows, settlementDate=ql.Date(), npvDate=ql.Date(), accuracy=1.0e-10, maxIterations=100, guess=0.0)

.. code-block:: python

    crv = ql.FlatForward(ql.Date(15,1,2020), 0.04, ql.Actual360())
    ql.CashFlows.zSpread(leg, 5.5, crv, ql.Actual360(), ql.Compounded, ql.Annual, True)







