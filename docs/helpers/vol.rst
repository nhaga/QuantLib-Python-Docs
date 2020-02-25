Volatility
##########

CapHelper
*********

.. function:: ql.CapHelper(period, quote, index, frequency, dayCounter, includeFirstOptionlet (bool), YieldTermStructure, errorType=BlackCalibrationHelper.RelativePriceError)

.. code-block:: python

  period = ql.Period('2y')
  quote = ql.QuoteHandle(ql.SimpleQuote(0.55))
  today = ql.Date().todaysDate()
  yts = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.02, ql.Actual360()))
  index = ql.Euribor6M(yts)

  helper = ql.CapHelper(period, quote, index, ql.Semiannual, ql.Actual360(), False, yts)


SwaptionHelper
**************

.. function:: ql.SwaptionHelper()
