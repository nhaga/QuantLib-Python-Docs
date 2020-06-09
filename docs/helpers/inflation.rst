Inflation
#########


ZeroCouponInflationSwapHelper
-----------------------------

.. function:: ql.ZeroCouponInflationSwapHelper(quote, period, date, calendar, convention, daycounter, index)

.. function:: ql.ZeroCouponInflationSwapHelper(quote, period, date, calendar, convention, daycounter, index, yieldTermStructure)

.. code-block:: python

  quote = ql.QuoteHandle(ql.SimpleQuote(0.02))
  period = ql.Period('6M')
  date = ql.Date(15,6,2020)
  calendar = ql.TARGET()
  convention = ql.ModifiedFollowing
  daycounter = ql.Actual360()
  index = ql.EUHICPXT(True)

  helper = ql.ZeroCouponInflationSwapHelper(quote, period, date, calendar, convention, daycounter, index)