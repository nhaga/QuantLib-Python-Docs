Volatility
##########


BlackConstantVol
****************

.. function:: ql.BlackConstantVol(date, calendar, volatility, dayCounter)

.. function:: ql.BlackConstantVol(date, calendar, volatilityHandle, dayCounter)

.. function:: ql.BlackConstantVol(days, calendar, volatility, dayCounter)

.. function:: ql.BlackConstantVol(days, calendar, volatilityHandle, dayCounter)

.. code-block:: python

  date = ql.Date().todaysDate()
  settlementDays = 2
  calendar = ql.TARGET()
  volatility = 0.2
  volHandle = ql.QuoteHandle(ql.SimpleQuote(volatility))
  dayCounter = ql.Actual360()

  ql.BlackConstantVol(date, calendar, volatility, dayCounter)
  ql.BlackConstantVol(date, calendar, volHandle, dayCounter)
  ql.BlackConstantVol(settlementDays, calendar, volatility, dayCounter)
  ql.BlackConstantVol(settlementDays, calendar, volHandle, dayCounter)


BlackVarianceCurve
******************

.. function:: ql.BlackVarianceCurve(referenceDate, expirations, volatilities, dayCounter)

.. code-block:: python

  referenceDate = ql.Date(30, 9, 2013)
  expirations = [ql.Date(20, 12, 2013), ql.Date(17, 1, 2014), ql.Date(21, 3, 2014)]
  volatilities = [.145, .156, .165]
  volatilityCurve = ql.BlackVarianceCurve(referenceDate, expirations, volatilities, ql.Actual360())

BlackVarianceSurface
********************

.. function:: ql.BlackVarianceSurface(referenceDate, calendar, expirations, strikes, volMatrix, dayCounter)

.. code-block:: python

  referenceDate = ql.Date(30, 9, 2013)
  ql.Settings.instance().evaluationDate = referenceDate;
  calendar = ql.TARGET()
  dayCounter = ql.ActualActual()

  strikes = [1650.0, 1660.0, 1670.0]
  expirations = [ql.Date(20, 12, 2013), ql.Date(17, 1, 2014), ql.Date(21, 3, 2014)]

  volMatrix = ql.Matrix(len(strikes), len(expirations))

  #1650 - Dec, Jan, Mar
  volMatrix[0][0] = .15640; volMatrix[0][1] = .15433; volMatrix[0][2] = .16079;
  #1660 - Dec, Jan, Mar
  volMatrix[1][0] = .15343; volMatrix[1][1] = .15240; volMatrix[1][2] = .15804;
  #1670 - Dec, Jan, Mar
  volMatrix[2][0] = .15128; volMatrix[2][1] = .14888; volMatrix[2][2] = .15512;

  volatilitySurface = ql.BlackVarianceSurface(
      referenceDate,
      calendar,
      expirations,
      strikes,
      volMatrix,
      dayCounter
  )
  volatilitySurface.enableExtrapolation()


HestonBlackVolSurface
*********************

.. function:: ql.HestonBlackVolSurface(hestonModelHandle)

.. code-block:: python

  flatTs = ql.YieldTermStructureHandle(
    ql.FlatForward(ql.Date().todaysDate(), 0.05, ql.Actual365Fixed())
  )
  dividendTs = ql.YieldTermStructureHandle(
    ql.FlatForward(ql.Date().todaysDate(), 0.02, ql.Actual365Fixed())
  )

  v0 = 0.01; kappa = 0.01; theta = 0.01; rho = 0.0; sigma = 0.01
  spot = 100
  process = ql.HestonProcess(flatTs, dividendTs,
                              ql.QuoteHandle(ql.SimpleQuote(spot)),
                              v0, kappa, theta, sigma, rho
                              )

  hestonModel = ql.HestonModel(process)
  hestonHandle = ql.HestonModelHandle(hestonModel)
  hestonVolSurface = ql.HestonBlackVolSurface(hestonHandle)


AndreasenHugeVolatilityAdapter
******************************

An implementation of the arb-free Andreasen-Huge vol interpolation described in "Andreasen J., Huge B., 2010. Volatility Interpolation" (https://ssrn.com/abstract=1694972). An advantage of this method is that it can take a non-rectangular grid of option quotes.

.. function:: ql.AndreasenHugeVolatilityAdapter(AndreasenHugeVolatilityInterpl)

.. code-block:: python

  today = ql.Date().todaysDate()
  calendar = ql.NullCalendar()
  dayCounter = ql.Actual365Fixed()
  spot = 100
  r, q = 0.02, 0.05

  spotQuote = ql.QuoteHandle(ql.SimpleQuote(spot))
  ratesTs = ql.YieldTermStructureHandle(ql.FlatForward(today, r, dayCounter))
  dividendTs = ql.YieldTermStructureHandle(ql.FlatForward(today, q, dayCounter))

  # Market options price quotes
  optionStrikes = [95, 97.5, 100, 102.5, 105, 90, 95, 100, 105, 110, 80, 90, 100, 110, 120]
  optionMaturities = ["3M", "3M", "3M", "3M", "3M", "6M", "6M", "6M", "6M", "6M", "1Y", "1Y", "1Y", "1Y", "1Y"]
  optionQuotedVols = [0.11, 0.105, 0.1, 0.095, 0.095, 0.12, 0.11, 0.105, 0.1, 0.105, 0.12, 0.115, 0.11, 0.11, 0.115]

  calibrationSet = ql.CalibrationSet()

  for strike, expiry, impliedVol in zip(optionStrikes, optionMaturities, optionQuotedVols):
    payoff = ql.PlainVanillaPayoff(ql.Option.Call, strike)
    exercise = ql.EuropeanExercise(calendar.advance(today, ql.Period(expiry)))

    calibrationSet.push_back((ql.VanillaOption(payoff, exercise), ql.SimpleQuote(impliedVol)))

  ahInterpolation = ql.AndreasenHugeVolatilityInterpl(calibrationSet, spotQuote, ratesTs, dividendTs)
  ahSurface = ql.AndreasenHugeVolatilityAdapter(ahInterpolation)


BlackVolTermStructureHandle
***************************

.. function:: ql.BlackVolTermStructureHandle(blackVolTermStructure)

.. code-block:: python

  ql.BlackVolTermStructureHandle(constantVol)
  ql.BlackVolTermStructureHandle(volatilityCurve)
  ql.BlackVolTermStructureHandle(volatilitySurface)

RelinkableBlackVolTermStructureHandle
*************************************

.. function:: ql.RelinkableBlackVolTermStructureHandle()

.. function:: ql.RelinkableBlackVolTermStructureHandle(blackVolTermStructure)

.. code-block:: python

  blackTSHandle = ql.RelinkableBlackVolTermStructureHandle(volatilitySurface)

  blackTSHandle = ql.RelinkableBlackVolTermStructureHandle()
  blackTSHandle.linkTo(volatilitySurface)


LocalConstantVol
****************

.. function:: ql.LocalConstantVol(date, volatility, dayCounter)

.. code-block:: python

  date = ql.Date().todaysDate()
  volatility = 0.2
  dayCounter = ql.Actual360()

  ql.LocalConstantVol(date, volatility, dayCounter)


LocalVolSurface
***************

.. function:: ql.LocalVolSurface(blackVolTs, ratesTs, dividendsTs, spot)

.. code-block:: python

  today = ql.Date().todaysDate()
  calendar = ql.NullCalendar()
  dayCounter = ql.Actual365Fixed()
  volatility = 0.2
  r, q = 0.02, 0.05

  blackVolTs = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, calendar, volatility, dayCounter))
  ratesTs = ql.YieldTermStructureHandle(ql.FlatForward(today, r, dayCounter))
  dividendTs = ql.YieldTermStructureHandle(ql.FlatForward(today, q, dayCounter))
  spot = 100

  ql.LocalVolSurface(blackVolTs, ratesTs, dividendTs, spot)


NoExceptLocalVolSurface
***********************

This powerful but dangerous surface will swallow any exceptions and return the specified override value when they occur. If your vol surface is well-calibrated, this protects you from crashes due to very far illiquid points on the local vol surface. But if your vol surface is not good, it could suppress genuine errors. Caution recommended.

.. function:: ql.NoExceptLocalVolSurface(blackVolTs, ratesTs, dividendsTs, spot, illegalVolOverride)

.. code-block:: python

  today = ql.Date().todaysDate()
  calendar = ql.NullCalendar()
  dayCounter = ql.Actual365Fixed()
  r, q = 0.02, 0.05
  volatility = 0.2
  illegalVolOverride = 0.25

  blackVolTs = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, calendar, volatility, dayCounter))
  ratesTs = ql.YieldTermStructureHandle(ql.FlatForward(today, r, dayCounter))
  dividendTs = ql.YieldTermStructureHandle(ql.FlatForward(today, q, dayCounter))
  spot = 100

  ql.NoExceptLocalVolSurface(blackVolTs, ratesTs, dividendTs, spot, illegalVolOverride)


AndreasenHugeLocalVolAdapter
****************************

.. function:: ql.AndreasenHugeLocalVolAdapter(AndreasenHugeVolatilityInterpl)

.. code-block:: python

  today = ql.Date().todaysDate()
  calendar = ql.NullCalendar()
  dayCounter = ql.Actual365Fixed()
  spot = 100
  r, q = 0.02, 0.05

  spotQuote = ql.QuoteHandle(ql.SimpleQuote(spot))
  ratesTs = ql.YieldTermStructureHandle(ql.FlatForward(today, r, dayCounter))
  dividendTs = ql.YieldTermStructureHandle(ql.FlatForward(today, q, dayCounter))

  # Market options price quotes
  optionStrikes = [95, 97.5, 100, 102.5, 105, 90, 95, 100, 105, 110, 80, 90, 100, 110, 120]
  optionMaturities = ["3M", "3M", "3M", "3M", "3M", "6M", "6M", "6M", "6M", "6M", "1Y", "1Y", "1Y", "1Y", "1Y"]
  optionQuotedVols = [0.11, 0.105, 0.1, 0.095, 0.095, 0.12, 0.11, 0.105, 0.1, 0.105, 0.12, 0.115, 0.11, 0.11, 0.115]

  calibrationSet = ql.CalibrationSet()

  for strike, expiry, impliedVol in zip(optionStrikes, optionMaturities, optionQuotedVols):
    payoff = ql.PlainVanillaPayoff(ql.Option.Call, strike)
    exercise = ql.EuropeanExercise(calendar.advance(today, ql.Period(expiry)))

    calibrationSet.push_back((ql.VanillaOption(payoff, exercise), ql.SimpleQuote(impliedVol)))

  ahInterpolation = ql.AndreasenHugeVolatilityInterpl(calibrationSet, spotQuote, ratesTs, dividendTs)
  ahLocalSurface = ql.AndreasenHugeLocalVolAdapter(ahInterpolation)


LocalVolTermStructureHandle
***************************

