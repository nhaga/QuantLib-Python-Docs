Inflation
#########

CPI Bond
********

.. function:: ql.CPIBond(settlementDays, notional, growthOnly, baseCPI, contractObservationLag, inflationIndex, observationInterpolation, fixedSchedule, fixedRates, fixedDayCounter, fixedPaymentConvention)

.. code-block:: python

  calendar = ql.UnitedKingdom()

  today = ql.Date(5,3,2008)
  evaluationDate = calendar.adjust(today)
  issue_date = calendar.advance(evaluationDate,-1, ql.Years)
  maturity_date = ql.Date(2,9,2052)

  settlementDays = 3
  notional = 1000000
  growthOnly = False
  baseCPI = 206.1
  contractObservationLag = ql.Period(3, ql.Months)
  inflationIndex = ql.UKRPI(False)
  observationInterpolation = ql.CPI.Flat
  fixedSchedule = ql.MakeSchedule(issue_date, maturity_date, ql.Period(ql.Semiannual))
  fixedRates = [0.1]    
  fixedDayCounter = ql.Actual365Fixed()
  fixedPaymentConvention = ql.ModifiedFollowing

  bond = ql.CPIBond(settlementDays,
                      notional,
                      growthOnly,
                      baseCPI,
                      contractObservationLag,
                      inflationIndex,
                      observationInterpolation,
                      fixedSchedule,
                      fixedRates,
                      fixedDayCounter, 
                      fixedPaymentConvention)



CPISwap
*******

.. function:: ql.CPISwap(swapType, nominal, subtractInflationNominal, spread, floatDayCount, schedule, floatPaymentConvention, fixingDays, floatIndex, fixedRate, baseCPI, fixedDayCount, schedule, fixedPaymentConvention, contractObservationLag, fixedIndex, observationInterpolation)

.. code-block:: python

  swapType = ql.CPISwap.Payer
  nominal = 1e6
  subtractInflationNominal = True
  spread = 0.0
  floatDayCount = ql.Actual365Fixed()
  floatPaymentConvention = ql.ModifiedFollowing
  fixingDays = 0;
  floatIndex = ql.GBPLibor(ql.Period('6M'))
  fixedRate = 0.1;
  baseCPI = 206.1;
  fixedDayCount = ql.Actual365Fixed()
  fixedPaymentConvention = ql.ModifiedFollowing;
  fixedIndex = ql.UKRPI(False);
  contractObservationLag = ql.Period('3M')
  observationInterpolation = ql.CPI.Linear
  startDate = ql.Date(2,10,2007)
  endDate = ql.Date(2,10,2052)
  schedule = ql.MakeSchedule(startDate, endDate, ql.Period('6m'))
  zisV = ql.CPISwap(
      swapType, nominal, subtractInflationNominal, spread, 
      floatDayCount, schedule, floatPaymentConvention, fixingDays, floatIndex,
      fixedRate, baseCPI, fixedDayCount, schedule, fixedPaymentConvention,
      contractObservationLag, fixedIndex, observationInterpolation)


ZeroCouponInflationSwap
***********************



YearOnYearInflationSwap
***********************

.. function:: ql.YearOnYearInflationSwap(swapType, nominal, fixedSchedule, fixedRate, fixedDayCounter, yoySchedule, index, lag, spread, yoyDayCounter, paymentCalendar)

.. code-block:: python

  swapType = ql.YearOnYearInflationSwap.Payer
  nominal = 1e6
  startDate = ql.Date(2,10,2007)
  endDate = ql.Date(2,10,2052)

  fixedSchedule = ql.MakeSchedule(startDate, endDate, ql.Period('6m'))
  fixedRate = 0.1;
  fixedDayCounter = ql.Actual365Fixed()
  yoySchedule = ql.MakeSchedule(startDate, endDate, ql.Period('6m'))
  index = ql.YYEUHICP(False)
  lag = ql.Period('3m')
  spread = 0.0
  yoyDayCounter = ql.Actual365Fixed()
  paymentCalendar = ql.TARGET()

  swap = ql.YearOnYearInflationSwap(swapType, nominal, fixedSchedule, fixedRate, fixedDayCounter, yoySchedule, index, lag, spread, yoyDayCounter, paymentCalendar)


YoYInflationCap
***************

YoYInflationFloor
*****************

YoYInflationCollar
******************