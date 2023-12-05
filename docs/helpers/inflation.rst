Inflation
#########


ZeroCouponInflationSwapHelper
-----------------------------

.. function:: ql.ZeroCouponInflationSwapHelper(quote, period, date, calendar, convention, daycounter, index, observationInterpolation, yieldTermStructure)

.. code-block:: python

  import QuantLib as ql

  quote = ql.QuoteHandle(ql.SimpleQuote(0.02))
  period = ql.Period('6M')
  date = ql.Date(15,6,2020)
  calendar = ql.TARGET()
  convention = ql.ModifiedFollowing
  daycounter = ql.Actual360()
  index = ql.EUHICPXT(True)
  
  flatForward = ql.FlatForward(ql.Date(15,6,2020), ql.QuoteHandle(ql.SimpleQuote(0.05)), ql.Actual360())
  yieldTermStructure = ql.YieldTermStructureHandle(flatForward)

  helper = ql.ZeroCouponInflationSwapHelper(quote, period, date, calendar, convention, daycounter, index, ql.CPI.Linear, yieldTermStructure)  