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

.. function:: ql.SwaptionHelper(maturity, length, volatility, index, fixedLegTenor, fixedLegDayCounter, floatingLegDayCounter, termStructure, errorType=ql.BlackCalibrationHelper.RelativePriceError, strike=Null< Real >(), nominal=1.0, type=ql.ShiftedLognormal, shift=0.0)

.. code-block:: python

  maturity = ql.Period('5Y')
  length = ql.Period('5Y')
  volatility = ql.QuoteHandle(ql.SimpleQuote(0.0055))
  index = ql.Euribor6M()
  fixedLegTenor = ql.Period('1Y')
  fixedLegDayCounter = ql.Thirty360()
  floatingLegDayCounter = ql.Actual360()

  crv = ql.FlatForward(2, ql.TARGET(), 0.05, ql.Actual360())
  yts = ql.YieldTermStructureHandle(crv)

  ql.SwaptionHelper(
    maturity, length, volatility, index, fixedLegTenor,
    fixedLegDayCounter, floatingLegDayCounter, yts
  )

.. function:: ql.SwaptionHelper (exerciseDate, length, volatility, index, fixedLegTenor, fixedLegDayCounter, floatingLegDayCounter, termStructure, errorType=ql.BlackCalibrationHelper.RelativePriceError, strike=Null< Real >(), nominal=1.0, type=ql.ShiftedLognormal, shift=0.0)

.. code-block:: python

  exerciseDate = ql.Date(15,6,2020)
  length = ql.Period('5Y')
  volatility = ql.QuoteHandle(ql.SimpleQuote(0.0055))
  index = ql.Euribor6M()
  fixedLegTenor = ql.Period('1Y')
  fixedLegDayCounter = ql.Thirty360()
  floatingLegDayCounter = ql.Actual360()

  crv = ql.FlatForward(2, ql.TARGET(), 0.05, ql.Actual360())
  yts = ql.YieldTermStructureHandle(crv)

  ql.SwaptionHelper(
    exerciseDate, length, volatility, index, fixedLegTenor,
    fixedLegDayCounter, floatingLegDayCounter, yts
  )


.. function:: ql.SwaptionHelper (exerciseDate, endDate, volatility, index, fixedLegTenor, fixedLegDayCounter, floatingLegDayCounter, termStructure, errorType=ql.BlackCalibrationHelper.RelativePriceError, strike=Null< Real >(), nominal=1.0, type=ql.ShiftedLognormal, shift=0.0)

.. code-block:: python

  exerciseDate = ql.Date(15,6,2020)
  endDate = ql.Date(15,6,2025)
  volatility = ql.QuoteHandle(ql.SimpleQuote(0.0055))
  index = ql.Euribor6M()
  fixedLegTenor = ql.Period('1Y')
  fixedLegDayCounter = ql.Thirty360()
  floatingLegDayCounter = ql.Actual360()

  crv = ql.FlatForward(2, ql.TARGET(), 0.05, ql.Actual360())
  yts = ql.YieldTermStructureHandle(crv)

  ql.SwaptionHelper(
    exerciseDate, endDate, volatility, index, fixedLegTenor,
    fixedLegDayCounter, floatingLegDayCounter, yts
  )


HestonModelHelper
*****************

.. function:: ql.HestonModelHelper(tenor, calendar, spot, strike, volQuote, riskFreeCurveHandle, dividendCurveHandle, errorType=ql.BlackCalibrationHelper.RelativePriceError)

.. code-block:: python

  spot, strike = 100, 110

  tenor = ql.Period("3M")
  calendar = ql.NullCalendar()
  dayCount = ql.Actual365Fixed()
  volQuote = ql.QuoteHandle(ql.SimpleQuote(0.22))

  today = ql.Date().todaysDate()
  riskFreeCurve = ql.FlatForward(today, 0.04, dayCount)
  dividendCurve = ql.FlatForward(today, 0.0, dayCount)
  riskFreeHandle = ql.YieldTermStructureHandle(riskFreeCurve)
  dividendHandle = ql.YieldTermStructureHandle(dividendCurve)

  ql.HestonModelHelper(tenor, calendar, spot, strike, volQuote, riskFreeHandle, dividendHandle)