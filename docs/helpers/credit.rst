Credit
######

SpreadCdsHelper
***************

.. function:: ql.SpreadCdsHelper(runningSpread, tenor, settlementDays, calendar, frequency, paymentConvention, rule, dayCounter, recoveryRate, discountCurve, settlesAccrual=True, paysAtDefaultTime=True, startDate=ql.Date(), lastPeriodDayCounter=ql.DayCounter(), rebatesAccrual=True, model=ql.CreditDefaultSwap.Midpoint)

.. code-block:: python

  runningSpread = ql.QuoteHandle(ql.SimpleQuote(0.005))
  tenor = ql.Period('5Y')
  settlementDays = 2
  calendar = ql.TARGET()
  frequency = ql.Annual
  paymentConvention = ql.Following
  rule = ql.DateGeneration.TwentiethIMM
  dayCounter = ql.Actual365Fixed()
  recoveryRate = 0.4

  crv = ql.FlatForward(2, ql.TARGET(), 0.05, ql.Actual360())
  yts = ql.YieldTermStructureHandle(crv)

  ql.SpreadCdsHelper(
    runningSpread, tenor, settlementDays, calendar, frequency,
    paymentConvention, rule, dayCounter, recoveryRate, yts
  )