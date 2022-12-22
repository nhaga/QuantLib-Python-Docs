
Interest Rate
#############

DepositRateHelper
*****************

.. function:: ql.DepositRateHelper (quote, tenor, fixingDays, calendar, convention, endOfMonth, dayCounter)

.. code-block:: python

  quote = ql.QuoteHandle(ql.SimpleQuote(0.05))
  tenor = ql.Period('6M')
  fixingDays = 2
  calendar = ql.TARGET()
  convention = ql.ModifiedFollowing
  endOfMonth = False
  dayCounter = ql.Actual360()
  ql.DepositRateHelper(quote, tenor, fixingDays, calendar, convention, endOfMonth, dayCounter)
  
.. function:: ql.DepositRateHelper(rate, tenor, fixingDays, calendar, convention, endOfMonth, dayCounter)

.. code-block:: python

  ql.DepositRateHelper(0.05, ql.Period('6M'), 2, ql.TARGET(), ql.ModifiedFollowing, False, ql.Actual360())


.. function:: ql.DepositRateHelper(quote, index)

.. code-block:: python

  ql.DepositRateHelper(ql.QuoteHandle(ql.SimpleQuote(0.05)), ql.Euribor6M())


.. function:: ql.DepositRateHelper(rate, index)


.. code-block:: python

  ql.DepositRateHelper(0.05, ql.Euribor6M());


FraRateHelper
*************

from months with quote
----------------------


.. function:: ql.FraRateHelper(quote, monthsToStart, monthsToEnd, fixingDays, calendar, convention, endOfMonth, dayCounter,pillar=ql.Pillar.LastRelevantDate, customPillarDate=ql.Date(), useIndexedCoupon=True)


.. code-block:: python

  quote = ql.QuoteHandle(ql.SimpleQuote(0.05))
  monthsToStart = 1
  monthsToEnd = 7
  fixingDays = 2
  calendar = ql.TARGET()
  convention = ql.ModifiedFollowing
  endOfMonth = False
  dayCounter = ql.Actual360()
  ql.FraRateHelper(quote, monthsToStart, monthsToEnd, fixingDays, calendar, convention, endOfMonth, dayCounter)
  ql.FraRateHelper(quote, monthsToStart, monthsToEnd, fixingDays, calendar, convention, endOfMonth, dayCounter, ql.Pillar.LastRelevantDate)
  ql.FraRateHelper(quote, monthsToStart, monthsToEnd, fixingDays, calendar, convention, endOfMonth, dayCounter, ql.Pillar.LastRelevantDate, ql.Date())
  ql.FraRateHelper(quote, monthsToStart, monthsToEnd, fixingDays, calendar, convention, endOfMonth, dayCounter, ql.Pillar.LastRelevantDate, ql.Date(), True)


from months with rate
---------------------


.. function:: ql.FraRateHelper(rate, monthsToStart, monthsToEnd, fixingDays, calendar, convention, endOfMonth, dayCounter,pillar=ql.Pillar.LastRelevantDate, customPillarDate=ql.Date(), useIndexedCoupon=True)

.. code-block:: python

  rate = 0.05
  monthsToStart = 1
  monthsToEnd = 7
  fixingDays = 2
  calendar = ql.TARGET()
  convention = ql.ModifiedFollowing
  endOfMonth = False
  dayCounter = ql.Actual360()
  ql.FraRateHelper(rate, monthsToStart, monthsToEnd, fixingDays, calendar, convention, endOfMonth, dayCounter)
  ql.FraRateHelper(rate, monthsToStart, monthsToEnd, fixingDays, calendar, convention, endOfMonth, dayCounter, ql.Pillar.LastRelevantDate)
  ql.FraRateHelper(rate, monthsToStart, monthsToEnd, fixingDays, calendar, convention, endOfMonth, dayCounter, ql.Pillar.LastRelevantDate, ql.Date())
  ql.FraRateHelper(rate, monthsToStart, monthsToEnd, fixingDays, calendar, convention, endOfMonth, dayCounter, ql.Pillar.LastRelevantDate, ql.Date(), True)



from quote, monthsToStart and index
-----------------------------------

.. function:: ql.FraRateHelper(quote, monthsToStart, index, pillar=ql.Pillar.LastRelevantDate, customPillarDate=ql.Date(), useIndexedCoupon=True)

.. code-block:: python

  quote = ql.QuoteHandle(ql.SimpleQuote(0.05))
  monthsToStart = 1
  index = ql.Euribor6M()
  ql.FraRateHelper(quote, monthsToStart, index)
  ql.FraRateHelper(quote, monthsToStart, index, ql.Pillar.LastRelevantDate)
  ql.FraRateHelper(quote, monthsToStart, index, ql.Pillar.LastRelevantDate, ql.Date())
  ql.FraRateHelper(quote, monthsToStart, index, ql.Pillar.LastRelevantDate, ql.Date(), True)

from price, monthsToStart and index
-----------------------------------

.. function:: ql.FraRateHelper(rate, monthsToStart, index, pillar=ql.Pillar.LastRelevantDate, customPillarDate=ql.Date(), useIndexedCoupon=True)

.. code-block:: python

  rate = 0.05
  monthsToStart = 1
  index = ql.Euribor6M()
  h = ql.FraRateHelper(rate, monthsToStart, index)
  ql.FraRateHelper(rate, monthsToStart, index, ql.Pillar.LastRelevantDate)
  ql.FraRateHelper(rate, monthsToStart, index, ql.Pillar.LastRelevantDate, ql.Date())
  ql.FraRateHelper(rate, monthsToStart, index, ql.Pillar.LastRelevantDate, ql.Date(), True)


from quote, immOffsets and index
--------------------------------

.. function:: ql.FraRateHelper(quote, immOffsetStart, immOffsetEnd, index, pillar=ql.Pillar.LastRelevantDate, customPillarDate=ql.Date(), useIndexedCoupon=True)

.. code-block:: python

  quote = ql.QuoteHandle(ql.SimpleQuote(0.05))
  immOffsetStart = 1
  immOffsetEnd = 2
  index = ql.Euribor6M()
  ql.FraRateHelper(quote, immOffsetStart, immOffsetEnd, index)
  ql.FraRateHelper(quote, immOffsetStart, immOffsetEnd, index, ql.Pillar.LastRelevantDate)
  ql.FraRateHelper(quote, immOffsetStart, immOffsetEnd, index, ql.Pillar.LastRelevantDate, ql.Date())
  ql.FraRateHelper(quote, immOffsetStart, immOffsetEnd, index, ql.Pillar.LastRelevantDate, ql.Date(), True)


Futures
*******

FuturesRateHelper
-----------------

.. function:: ql.FuturesRateHelper(price, iborStartDate, iborIndex, convexityAdjustment=0.0, type=ql.Futures.IMM)

.. code-block:: python

  price = 100
  index = ql.Euribor3M()
  iborStartDate = ql.Date(17,6,2020)
  ql.FuturesRateHelper(price, iborStartDate, index)
  ql.FuturesRateHelper(price, iborStartDate, index, 0.01)
  ql.FuturesRateHelper(price, iborStartDate, index, 0.01, ql.Futures.IMM)
  ql.FuturesRateHelper(price, ql.Date(8,5,2020), index, 0.01, ql.Futures.ASX)

.. function:: ql.FuturesRateHelper(quote, iborStartDate, iborIndex, convexityAdjustment=ql.QuoteHandle(), type=ql.Futures.IMM)

.. code-block:: python

  quote = ql.QuoteHandle(ql.SimpleQuote(100))
  index = ql.Euribor3M()
  iborStartDate = ql.Date(17,6,2020)
  convexityAdjustment = ql.QuoteHandle(ql.SimpleQuote(0.01))
  ql.FuturesRateHelper(quote, iborStartDate, index)
  ql.FuturesRateHelper(quote, iborStartDate, index, convexityAdjustment)
  ql.FuturesRateHelper(quote, iborStartDate, index, convexityAdjustment, ql.Futures.IMM)
  ql.FuturesRateHelper(quote, ql.Date(8,5,2020), index, convexityAdjustment, ql.Futures.ASX)

.. function:: ql.FuturesRateHelper(price, iborStartDate, lengthInMonths, calendar, convention, endOfMonth, dayCounter, convexityAdjustment=0.0, type=ql.Futures.IMM)

.. code-block:: python

  price = 100
  iborStartDate = ql.Date(17,6,2020)
  lengthInMonths = 3
  calendar = ql.TARGET()
  convention = ql.Following
  endOfMonth = False
  dayCounter = ql.Actual360()
  ql.FuturesRateHelper (price, iborStartDate, lengthInMonths, calendar, convention, endOfMonth, dayCounter)
  ql.FuturesRateHelper (price, iborStartDate, lengthInMonths, calendar, convention, endOfMonth, dayCounter, 0.01)
  ql.FuturesRateHelper (price, iborStartDate, lengthInMonths, calendar, convention, endOfMonth, dayCounter, 0.01, ql.Futures.IMM)

.. function:: ql.FuturesRateHelper(price, iborStartDate, lengthInMonths, calendar, convention, endOfMonth, dayCounter, convexityAdjustment=0.0, type=ql.Futures.IMM)

.. code-block:: python

  quote = ql.QuoteHandle(ql.SimpleQuote(100))
  iborStartDate = ql.Date(17,6,2020)
  lengthInMonths = 3
  calendar = ql.TARGET()
  convention = ql.Following
  endOfMonth = False
  dayCounter = ql.Actual360()
  convexityAdjustment = ql.QuoteHandle(ql.SimpleQuote(0.01))
  ql.FuturesRateHelper (quote, iborStartDate, lengthInMonths, calendar, convention, endOfMonth, dayCounter)
  ql.FuturesRateHelper (quote, iborStartDate, lengthInMonths, calendar, convention, endOfMonth, dayCounter, convexityAdjustment)
  ql.FuturesRateHelper (quote, iborStartDate, lengthInMonths, calendar, convention, endOfMonth, dayCounter, convexityAdjustment, ql.Futures.IMM)

.. function:: ql.FuturesRateHelper(price, iborStartDate, iborEndDate, dayCounter, convexityAdjustment=0.0, ql.Futures.IMM)

.. code-block:: python

  price = 100
  iborStartDate = ql.Date(17,6,2020)
  iborEndDate = ql.Date(17,9,2020)
  dayCounter = ql.Actual360()
  ql.FuturesRateHelper (price, iborStartDate, iborEndDate, dayCounter)
  ql.FuturesRateHelper (price, iborStartDate, iborEndDate, dayCounter, 0.01)
  ql.FuturesRateHelper (price, iborStartDate, iborEndDate, dayCounter, 0.01, ql.Futures.IMM)

.. function:: ql.FuturesRateHelper(quote, iborStartDate, iborEndDate, dayCounter, convexityAdjustment=ql.QuoteHandle(), ql.Futures.IMM)

.. code-block:: python

  quote = ql.QuoteHandle(ql.SimpleQuote(100))
  iborStartDate = ql.Date(17,6,2020)
  iborEndDate = ql.Date(17,9,2020)
  dayCounter = ql.Actual360()
  convexityAdjustment = ql.QuoteHandle(ql.SimpleQuote(0.01))
  ql.FuturesRateHelper (quote, iborStartDate, iborEndDate, dayCounter)
  ql.FuturesRateHelper (quote, iborStartDate, iborEndDate, dayCounter, convexityAdjustment)
  ql.FuturesRateHelper (quote, iborStartDate, iborEndDate, dayCounter, convexityAdjustment, ql.Futures.IMM)

OvernightIndexFutureRateHelper
------------------------------

.. function:: ql.OvernightIndexFutureRateHelper(quote, valueDate, maturityDate, overnightIndex, convexityAdjustmentQuote=ql.QuoteHandle(), nettingType=ql.OvernightIndexFuture.Compounding)

Netting Types:

- Averaging
- Compounding

.. code-block:: python

  overnightIndex = ql.FedFunds()
  priceQuote = ql.QuoteHandle(ql.SimpleQuote(99.92))
  valueDate = ql.Date(3, 7, 2017)
  maturityDate = ql.Date(30, 6, 2020 )
  convexityAdjustment = ql.QuoteHandle()
  netting = ql.OvernightIndexFuture.Averaging
  future = ql.OvernightIndexFutureRateHelper(priceQuote, valueDate, maturityDate, overnightIndex)
  future = ql.OvernightIndexFutureRateHelper(priceQuote, valueDate, maturityDate, overnightIndex, convexityAdjustment, netting)




SofrFutureRateHelper
--------------------

.. function:: ql.SofrFutureRateHelper(price, month, year, frequency, index)

.. function:: ql.SofrFutureRateHelper(priceQuote, month, year, frequency, index)

.. code-block:: python

  price = 99.915
  ql.SofrFutureRateHelper(price, 3, 2020, ql.Quarterly, ql.Sofr())

  priceQuote = ql.QuoteHandle(ql.SimpleQuote(price))
  ql.SofrFutureRateHelper(priceQuote, 3, 2020, ql.Quarterly, ql.Sofr())

.. function:: ql.SofrFutureRateHelper(price, month, year, frequency, index, convexityAdjustment=0)

.. function:: ql.SofrFutureRateHelper(priceQuote, month, year, frequency, index, convexityAdjustmentQuote=ql.QuoteHandle())

.. code-block:: python

  price = 99.915
  convexityAdjustment = 0.004
  ql.SofrFutureRateHelper(price, 3, 2020, ql.Quarterly, ql.Sofr(), convexityAdjustment)

  priceQuote = ql.QuoteHandle(ql.SimpleQuote(price))
  convexityAdjustmentQuote = ql.QuoteHandle(ql.SimpleQuote(convexityAdjustment))
  ql.SofrFutureRateHelper(priceQuote, 3, 2020, ql.Quarterly, ql.Sofr(), convexityAdjustmentQuote)


.. function:: ql.SofrFutureRateHelper(price, month, year, frequency, index, convexityAdjustment=0, nettingType=ql.OvernightIndexFuture.Compounding)

.. function:: ql.SofrFutureRateHelper(priceQuote, month, year, frequency, index, convexityAdjustment=0, nettingType=ql.OvernightIndexFuture.Compounding)

Netting Types:

- Averaging
- Compounding

.. code-block:: python

  price = 99.915
  convexityAdjustment = 0.004
  ql.SofrFutureRateHelper(price, 3, 2020, ql.Quarterly, ql.Sofr(), 0.004, ql.OvernightIndexFuture.Averaging)

  priceQuote = ql.QuoteHandle(ql.SimpleQuote(price))
  convexityAdjustmentQuote = ql.QuoteHandle(ql.SimpleQuote(convexityAdjustment))

  ql.SofrFutureRateHelper(priceQuote,3,2020,ql.Quarterly, ql.Sofr(), convexityAdjustmentQuote, ql.OvernightIndexFuture.Compounding)


IMM 
---

(Not a helper)

.. function:: ql.IMM.date(codeString, date=ql.Date())`

.. code-block:: python

  ql.IMM.date('M0')
  ql.IMM.date('M0', ql.Date(20,6,2020))

.. function:: ql.IMM.code(immDate)

.. code-block:: python

  immDate = ql.Date(16,12,2020)
  ql.IMM.code(immDate)

.. function:: ql.IMM.isIMMcode(codeString, mainCycle=True)

.. code-block:: python

  ql.IMM.isIMMcode('M0')
  ql.IMM.isIMMcode('H0', True)

.. function:: ql.IMM.isIMMdate(date, mainCycle=True)

.. code-block:: python

  dt = ql.Date(15,1,2020)
  ql.IMM.isIMMdate(dt, True)

.. code-block:: python

  dates = ql.MakeSchedule(ql.Date(16,3,2020), ql.Date(16,12,2020), ql.Period('1M'))
  list(map(ql.IMM.isIMMdate, dates))

.. function:: ql.IMM.nextCode()

.. function:: ql.IMM.nextCode(date)

.. function:: ql.IMM.nextCode(date, mainCycle=True)

.. function:: ql.IMM.nextCode(codeString)

.. function:: ql.IMM.nextCode(codeString, mainCycle=True)

.. function:: ql.IMM.nextCode(codeString, mainCycle=True, refDate)

.. code-block:: python

  ql.IMM.nextCode()
  ql.IMM.nextCode(ql.Date(7,5,2020))
  ql.IMM.nextCode(ql.Date(7,5,2020), False)
  ql.IMM.nextCode('K0')
  ql.IMM.nextCode('K0', False)
  ql.IMM.nextCode('M9', False, ql.Date(16,8,2019))

.. function:: ql.IMM.nextDate()

.. function:: ql.IMM.nextDate(date)

.. function:: ql.IMM.nextDate(date, mainCycle=True)

.. function:: ql.IMM.nextDate(codeString)

.. function:: ql.IMM.nextDate(codeString, mainCycle=True)

.. function:: ql.IMM.nextDate(codeString, mainCycle=True, refDate)

.. code-block:: python

  ql.IMM.nextDate()
  ql.IMM.nextDate(ql.Date(7,5,2020))
  ql.IMM.nextDate(ql.Date(7,5,2020), False)
  ql.IMM.nextDate('K0')
  ql.IMM.nextDate('K0', False)
  ql.IMM.nextDate('M9', False, ql.Date(16,8,2019))

SwapRateHelper
**************

.. function:: ql.SwapRateHelper(rate, swapIndex, spread=0, fwdStart=ql.Period(), discountingCurve=ql.YieldTermStructureHandle, pillar=ql.Pillar.LastRelevantDate, customPillarDate=ql.Date(), endOfMonth=Dalse)

.. code-block:: python

  rate = 0.05
  swapIndex = ql.EuriborSwapIsdaFixA(ql.Period('1y'))
  spread = ql.QuoteHandle(ql.SimpleQuote(0.0))
  ql.SwapRateHelper(rate, ql.EuriborSwapIsdaFixA(ql.Period('1y')))
  ql.SwapRateHelper(rate, ql.EuriborSwapIsdaFixA(ql.Period('1y')), spread)
  ql.SwapRateHelper(rate, ql.EuriborSwapIsdaFixA(ql.Period('1y')), spread, ql.Period('1M'))
  discountCurve = ql.YieldTermStructureHandle(ql.FlatForward(2, ql.TARGET(), 0.05, ql.Actual360()))
  ql.SwapRateHelper(rate, ql.EuriborSwapIsdaFixA(ql.Period('1y')), spread, ql.Period(), discountCurve)

.. function:: ql.SwapRateHelper(quote, tenor, calendar, fixedFrequency, fixedConvention, fixedDayCount, iborIndex, spread=ql.QuoteHandle(), fwdStart=ql.Period(), discountingCurve=ql.YieldTermStructureHandle(),     settlementDays=Null< Natural >(), pillar=ql.Pillar.LastRelevantDate, customPillarDate=ql.Date(), endOfMonth=False)

.. code-block:: python

  quote = ql.QuoteHandle(ql.SimpleQuote(0.05))
  swapIndex = ql.EuriborSwapIsdaFixA(ql.Period('1y'))
  spread = ql.QuoteHandle(ql.SimpleQuote(0.0))
  ql.SwapRateHelper(quote, ql.EuriborSwapIsdaFixA(ql.Period('1y')))
  ql.SwapRateHelper(quote, ql.EuriborSwapIsdaFixA(ql.Period('1y')), spread)
  ql.SwapRateHelper(quote, ql.EuriborSwapIsdaFixA(ql.Period('1y')), spread, ql.Period('1M'))
  discountCurve = ql.YieldTermStructureHandle(ql.FlatForward(2, ql.TARGET(), 0.05, ql.Actual360()))
  ql.SwapRateHelper(quote, ql.EuriborSwapIsdaFixA(ql.Period('1y')), spread, ql.Period(), discountCurve)

.. function:: ql.SwapRateHelper(quote, tenor, calendar, fixedFrequency, fixedConvention, fixedDayCount, iborIndex, spread=ql.QuoteHandle(), fwdStart=ql.Period(), discountingCurve=ql.YieldTermStructureHandle(), settlementDays, pillar=ql.Pillar.LastRelevantDate, customPillarDate=ql.Date(), endOfMonth=False)

.. code-block:: python

  rate = ql.QuoteHandle(ql.SimpleQuote(0.05))
  tenor = ql.Period('5Y')
  fixedFrequency = ql.Annual
  fixedConvention = ql.Following
  fixedDayCount = ql.Thirty360()
  iborIndex = ql.Euribor6M()
  ql.SwapRateHelper(rate, tenor, calendar, fixedFrequency, fixedConvention, fixedDayCount, iborIndex)

.. function:: ql.SwapRateHelper(rate, tenor, calendar, fixedFrequency, fixedConvention, fixedDayCount, iborIndex, spread=ql.QuoteHandle(), fwdStart=ql.Period(), discountingCurve=ql.YieldTermStructureHandle(), settlementDays, pillar=ql.Pillar.LastRelevantDate, customPillarDate=ql.Date(), endOfMonth=False)
 
.. code-block:: python

  rate = 0.05
  tenor = ql.Period('5Y')
  fixedFrequency = ql.Annual
  fixedConvention = ql.Following
  fixedDayCount = ql.Thirty360()
  iborIndex = ql.Euribor6M()
  ql.SwapRateHelper(rate, tenor, calendar, fixedFrequency, fixedConvention, fixedDayCount, iborIndex)



OISRateHelper
*************

.. function:: ql.OISRateHelper(settlementDays, tenor, fixedRate, overnightIndex, discountingCurve=ql.YieldTermStructureHandle(), telescopicValueDates=False, paymentLag=0, paymentConvention=ql.Following, paymentFrequency=ql.Annual, paymentCalendar=ql.Calendar(), forwardStart=ql.Period(), overnightSpread=0.0, pillar=ql.Pillar.LastRelevantDate, customPillarDate=qlDate() )

.. code-block:: python

  forward6mLevel = 0.025
  forward6mQuote = ql.QuoteHandle(ql.SimpleQuote(forward6mLevel))
  yts6m = ql.FlatForward(0, ql.TARGET(), forward6mQuote, ql.Actual365Fixed() )
  yts6mh = ql.YieldTermStructureHandle(yts6m)
  oishelper = ql.OISRateHelper(2,ql.Period("3M"), ql.QuoteHandle(ql.SimpleQuote(0.01)), ql.Eonia(yts6mh),yts6mh, True)

DatedOISRateHelper
******************

.. function:: ql.DatedOISRateHelper (startDate, endDate, fixedRate, overnightIndex, discountingCurve=ql. YieldTermStructureHandle(), telescopicValueDates=False)

.. code-block:: python

  startDate = ql.Date(15,6,2020)
  endDate = ql.Date(15,6,2021)
  fixedRate = ql.QuoteHandle(ql.SimpleQuote(0.05))
  overnightIndex = ql.Eonia()
  ql.DatedOISRateHelper(startDate, endDate, fixedRate, overnightIndex)


FxSwapRateHelper
****************


.. function:: ql.FxSwapRateHelper (fwdPoint, spotFx, tenor, fixingDays, calendar, convention, endOfMonth, isFxBaseCurrencyCollateralCurrency, collateralCurve)

.. function:: ql.FxSwapRateHelper (fwdPoint, spotFx, tenor, fixingDays, calendar, convention, endOfMonth, isFxBaseCurrencyCollateralCurrency, collateralCurve, tradingCalendar=Calendar())

.. code-block:: python

  yts = ql.YieldTermStructureHandle(ql.FlatForward(2, ql.TARGET(), 0.02, ql.Actual360()))
  spot = ql.QuoteHandle(ql.SimpleQuote(1.10))
  fwdPoints = ql.QuoteHandle(ql.SimpleQuote(122.29))
  ql.FxSwapRateHelper(fwdPoints, spot, ql.Period('6M'), 2, ql.TARGET(), ql.Following, False, True, yts)



CrossCurrencyBasisSwapRateHelper
********************************

.. function:: ql.CrossCurrencyBasisSwapRateHelper(basis, tenor, fixingDays, calendar, convention, endOfMonth, baseCurrencyIndex, quoteCurrencyIndex, collateralCurve, isFxBaseCurrencyCollateralCurrency, isBasisOnFxBaseCurrencyLeg)

.. code-block:: python

  eur_curve = ql.YieldTermStructureHandle(ql.FlatForward(2, ql.TARGET(), 0.01, ql.Actual360()))
  usd_curve = ql.YieldTermStructureHandle(ql.FlatForward(2, ql.TARGET(), 0.02, ql.Actual360()))

  basis = ql.QuoteHandle(ql.SimpleQuote(0.005))
  tenor = ql.Period('1Y')
  fixingDays = 2
  calendar = ql.TARGET()
  convention = ql.ModifiedFollowing
  endOfMonth = True
  baseCurrencyIndex = ql.USDLibor(ql.Period('3M'), usd_curve)
  quoteCurrencyIndex = ql.Euribor3M(eur_curve)
  collateralCurve = ql.YieldTermStructureHandle(ql.FlatForward(2, ql.TARGET(), 0.05, ql.Actual360()))
  isFxBaseCurrencyCollateralCurrency = False
  isBasisOnFxBaseCurrencyLeg = False

  helper = ql.CrossCurrencyBasisSwapRateHelper(
      basis, tenor, fixingDays, calendar, convention, endOfMonth,
      baseCurrencyIndex, quoteCurrencyIndex, collateralCurve,
      isFxBaseCurrencyCollateralCurrency, isBasisOnFxBaseCurrencyLeg)



FixedRateBondHelper
*******************

.. function:: ql.FixedRateBondHelper (price, settlementDays, faceAmount, schedule, coupons, dayCounter, paymentConv=Following, redemption=100.0, issueDate=Date(), paymentCalendar=Calendar(), exCouponPeriod=Period(), exCouponCalendar=Calendar(), exCouponConvention=Unadjusted, exCouponEndOfMonth=False, useCleanPrice=True)

.. code-block:: python

  quote = ql.QuoteHandle(ql.SimpleQuote(115.5))
  settlementDays = 2
  faceAmount = 100
  schedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(15,6,2021), ql.Period('1y'))
  coupons = [0.0195]
  dayCounter = ql.Actual360()
  helper = ql.FixedRateBondHelper(quote, settlementDays, faceAmount, schedule, coupons, dayCounter)

BondHelper
**********

.. function:: ql.BondHelper(cleanPrice, bond, useCleanPrice=True)

.. code-block:: python

  bond = ql.FixedRateBond(
      2, ql.TARGET(), 100.0, ql.Date(15,12,2019), ql.Date(15,12,2024),
      ql.Period('1Y'), [0.05], ql.Actual360())

  cleanPrice = ql.QuoteHandle(ql.SimpleQuote(115))
  ql.BondHelper(cleanPrice, bond)

BondHelperVector
****************

.. function:: ql.BondHelperVector()

.. code-block:: python

  bond_helpers = ql.BondHelperVector()
  bond_helpers.append(bond_helper)


RateHelperVector
****************

.. function:: ql.RateHelperVector()

.. code-block:: python

  helpers = ql.RateHelperVector()
  helpers.append(ql.DepositRateHelper(0.05, ql.Euribor6M()))

