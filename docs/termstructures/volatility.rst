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



HestonBlackVolSurface
*********************

.. function:: ql.HestonBlackVolSurface(hestonModelHandle)

.. code-block:: python

  flat_ts = ql.YieldTermStructureHandle(
    ql.FlatForward(ql.Date().todaysDate(), 0.05, ql.Actual365Fixed())
  )
  dividend_ts = ql.YieldTermStructureHandle(
    ql.FlatForward(ql.Date().todaysDate(), 0.02, ql.Actual365Fixed())
  )
  v0 = 0.01; kappa = 0.01; theta = 0.01; rho = 0.0; sigma = 0.01
  spot = 100
  process = ql.HestonProcess(flat_ts, dividend_ts,
                              ql.QuoteHandle(ql.SimpleQuote(spot)),
                              v0, kappa, theta, sigma, rho
                              )
  heston_model = ql.HestonModel(process)
  heston_handle = ql.HestonModelHandle(heston_model)
  heston_vol_surface = ql.HestonBlackVolSurface(heston_handle)


LocalConstantVol
****************

LocalVolSurface
***************

LocalVolTermStructureHandle
***************************

