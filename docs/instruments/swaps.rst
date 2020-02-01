Swaps
*****

VanillaSwap
-----------

.. function:: ql.VanillaSwap(type, nominal, fixedSchedule, fixedRate, fixedDayCount, floatSchedule, index, spread, floatingDayCount)


Types:

* `ql.VanillaSwap.Payer`
* `ql.VanillaSwap.Receiver`


.. code-block:: python

  calendar = ql.TARGET()
  start = ql.Date(17,6,2019)
  maturity = calendar.advance(start, ql.Period('5y'))

  fixedSchedule = ql.MakeSchedule(start, maturity, ql.Period('1Y'))

  floatSchedule = ql.MakeSchedule(start, maturity, ql.Period('6M'))

  swap = ql.VanillaSwap(
      ql.VanillaSwap.Payer, 100,
      fixedSchedule, 0.01, ql.Thirty360(),
      floatSchedule, ql.Euribor6M(), 0, ql.Actual360()
  )


Swap
----

.. function:: ql.Swap(firstLeg, secondLeg)

.. code-block:: python

  fixedSchedule = ql.MakeSchedule(start, maturity, ql.Period('1Y'))
  fixedLeg = ql.FixedRateLeg(fixedSchedule, ql.Actual360(), [100], [0.01])

  floatSchedule = ql.MakeSchedule(start, maturity, ql.Period('6M'))
  floatLeg = ql.IborLeg([100], floatSchedule, ql.Euribor6M(), ql.Actual360())

  swap = ql.Swap(fixedLeg, floatLeg)


MakeVanillaSwap
---------------

.. function:: ql.MakeVanillaSwap(tenor, index, fixedRate, forwardStart)


**Optional params:**

- fixedLegDayCount
- Nominal
- receiveFixed, 
- swapType
- settlementDays
- effectiveDate
- terminationDate
- dateGenerationRule
- fixedLegTenor
- fixedLegCalendar
- fixedLegConvention
- fixedLegDayCount
- floatingLegTenor
- floatingLegCalendar
- floatingLegConvention
- floatingLegDayCount
- floatingLegSpread
- discountingTermStructure
- pricingEngine
- fixedLegTerminationDateConvention
- fixedLegDateGenRule
- fixedLegEndOfMonth
- fixedLegFirstDate
- fixedLegNextToLastDate,
- floatingLegTerminationDateConvention
- floatingLegDateGenRule
- floatingLegEndOfMonth
- floatingLegFirstDate
- floatingLegNextToLastDate

.. code-block:: python

  tenor = ql.Period('5y')
  index = ql.Euribor6M()
  fixedRate = 0.05
  forwardStart = ql.Period("2D")

  swap = ql.MakeVanillaSwap(tenor, index, fixedRate, forwardStart, Nominal=100)
  swap = ql.MakeVanillaSwap(tenor, index, fixedRate, forwardStart, swapType=ql.VanillaSwap.Payer)


Amortizing Swap
---------------

.. code-block:: python


  calendar = ql.TARGET()
  start = ql.Date(17,6,2019)
  maturity = calendar.advance(start, ql.Period('2y'))


  fixedSchedule = ql.MakeSchedule(start, maturity, ql.Period('1Y'))
  fixedLeg = ql.FixedRateLeg(fixedSchedule, ql.Actual360(), [100, 50], [0.01])

  floatSchedule = ql.MakeSchedule(start, maturity, ql.Period('6M'))
  floatLeg = ql.IborLeg([100, 100, 50, 50], floatSchedule, ql.Euribor6M(), ql.Actual360())

  swap = ql.Swap(fixedLeg, floatLeg)


FloatFloatSwap
--------------

.. code-block:: python

  ql.FloatFloatSwap(ql.VanillaSwap.Payer,
                  [notional] * (len(float3m)-1),
                  [notional] * (len(float6m)-1),
                  float3m,
                  index3m,
                  ql.Actual360(),
                  float6m,
                  index6m,
                  ql.Actual360(), False, False,
                  [1] * (len(float3m)-1),
                  [spread] * (len(float3m)-1))


AssetSwap
---------

.. function:: ql.AssetSwap(payFixed, bond, cleanPrice, index, spread)

.. function:: ql.AssetSwap(payFixed, bond, cleanPrice, index, spread, schedule, dayCount, bool)

.. code-block:: python

  payFixedRate = True
  bond = ql.FixedRateBond(2, ql.TARGET(), 100.0, ql.Date(15,12,2019), ql.Date(15,12,2024),
      ql.Period('1Y'), [0.05], ql.ActualActual()
      )
  bondCleanPrice = 100
  index = ql.Euribor6M()
  spread = 0.0
  ql.AssetSwap(payFixedRate, bond, bondCleanPrice, index, spread, ql.Schedule(), ql.ActualActual(), True)


OvernightIndexedSwap
--------------------

.. function:: ql.OvernightIndexedSwap(swapType, nominal, schedule, fixedRate, fixedDC, overnightIndex)

Or array of nominals

.. function:: ql.OvernightIndexedSwap(swapType, nominals, schedule, fixedRate, fixedDC, overnightIndex)

Optional params:

- spread=0.0
- paymentLag=0
- paymentAdjustment=ql.Following()
- paymentCalendar=ql.Calendar()
- telescopicValueDates=false

Types:

- `ql.OvernightIndexedSwap.Receiver`
- `ql.OvernightIndexedSwap.Receiver`

.. code-block:: python

  swapType = ql.OvernightIndexedSwap.Receiver
  nominal = 100
  schedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(15,6,2021), ql.Period('1d'), calendar=ql.TARGET())
  fixedRate = 0.01
  fixedDC = ql.Actual360()
  overnightIndex = ql.Eonia()
  ois_swap = ql.OvernightIndexedSwap(swapType, nominal, schedule, fixedRate, fixedDC, overnightIndex)

MakeOIS
-------

.. function:: ql.MakeOIS(swapTenor, overnightIndex, fixedRate)


Optional params:

- fwdStart=Period(0, Days)
- receiveFixed=True,
- swapType=OvernightIndexedSwap.Payer
- nominal=1.0
- settlementDays=2
- effectiveDate=None
- terminationDate=None
- dateGenerationRule=DateGeneration.Backward
- paymentFrequency=Annual
- paymentAdjustmentConvention=Following
- paymentLag=0
- paymentCalendar=None
- endOfMonth=True
- fixedLegDayCount=None
- overnightLegSpread=0.0
- discountingTermStructure=None
- telescopicValueDates=False
- pricingEngine=None

.. code-block:: python

  swapTenor = ql.Period('1Y')
  overnightIndex = ql.Eonia()
  fixedRate = 0.01
  ois_swap = ql.MakeOIS(swapTenor, overnightIndex, fixedRate)

NonstandardSwap
---------------

.. function:: ql.NonstandardSwap(swapType, fixedNominal, floatingNominal, fixedSchedule, fixedRate, fixedDayCount, floatingSchedule, iborIndex, gearing, spread, floatDayCount)

Optional params:

- intermediateCapitalExchange = False
- finalCapitalExchange = False,
- paymentConvention = None

.. code-block:: python

  swapType = ql.VanillaSwap.Payer
  fixedNominal = [100, 100]
  floatingNominal  = [100] * 4
  fixedSchedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(15,6,2022), ql.Period('1Y'))
  fixedRate = [0.02] * 2
  fixedDayCount = ql.Thirty360()
  floatingSchedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(15,6,2022), ql.Period('6M'))
  iborIndex = ql.Euribor6M()
  gearing = [1.] * 4
  spread = [0.] * 4
  floatDayCount = iborIndex.dayCounter()
  nonstandardSwap = ql.NonstandardSwap(
      swapType, fixedNominal, floatingNominal,
      fixedSchedule, fixedRate, fixedDayCount,
      floatingSchedule, iborIndex, gearing, spread, floatDayCount)


