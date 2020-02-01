Swaption Volatility
###################

ConstantSwaptionVolatility
**************************

Constant swaption volatility, no time-strike dependence.

**floating reference date, floating market data**

.. function:: ql.ConstantSwaptionVolatility(settlementDays, cal, bdc, volatility, dc, type=ql.ShiftedLognormal, shift=0.0)

**fixed reference date, floating market data**

.. function:: ql.ConstantSwaptionVolatility(settlementDate, cal, bdc, volatility, dc, type=ql.ShiftedLognormal, shift=0.0)

**floating reference date, fixed market data**

.. function:: ql.ConstantSwaptionVolatility(settlementDays, cal, bdc, volatilityQuote, dc, type=ql.ShiftedLognormal, shift=0.0)

**fixed reference date, fixed market data**

.. function:: ql.ConstantSwaptionVolatility(settlementDate, cal, bdc, volatilityQuote, dc, type=ql.ShiftedLognormal, shift=0.0)

.. code-block:: python

  constantSwaptionVol = ql.ConstantSwaptionVolatility(2, ql.TARGET(), ql.ModifiedFollowing, ql.QuoteHandle(ql.SimpleQuote(0.55)), ql.ActualActual())

SwaptionVolatilityMatrix
************************

At-the-money swaption-volatility matrix.

**floating reference date, floating market data**

.. function:: ql.SwaptionVolatilityMatrix(calendar, bdc, optionTenors, swapTenors, vols (Handles), dayCounter, flatExtrapolation=false, type=ShiftedLognormal, shifts (vector))

fixed reference date, floating market data

.. function:: ql.SwaptionVolatilityMatrix(referenceDate, calendar, bdc, optionTenors, swapTenors, vols (Handles), dayCounter, flatExtrapolation=false, type=ShiftedLognormal, shifts (vector))

floating reference date, fixed market data

.. function:: ql.SwaptionVolatilityMatrix(calendar, bdc, optionTenors, swapTenors, vols (matrix), dayCounter, flatExtrapolation=false, type=ShiftedLognormal, shifts (matrix))

fixed reference date, fixed market data

.. function:: ql.SwaptionVolatilityMatrix(referenceDate, calendar, bdc, optionTenors, swapTenors, vols (matrix), dayCounter, flatExtrapolation=false, type=ShiftedLognormal, shifts (matrix))

fixed reference date and fixed market data, option dates

.. function:: ql.SwaptionVolatilityMatrix(referenceDate, calendar, bdc, optionDates, swapTenors, vols (matrix), dayCounter, flatExtrapolation=false, type=ShiftedLognormal, shifts (matrix))


.. code-block:: python

  # market Data 07.01.2020

  swapTenors = [
      '1Y', '2Y', '3Y', '4Y', '5Y',
      '6Y', '7Y', '8Y', '9Y', '10Y',
      '15Y', '20Y', '25Y', '30Y']

  optionTenors = [
      '1M', '2M', '3M', '6M', '9M', '1Y',
      '18M', '2Y', '3Y', '4Y', '5Y', '7Y',
      '10Y', '15Y', '20Y', '25Y', '30Y']

  normal_vols = [
      [8.6, 12.8, 19.5, 26.9, 32.7, 36.1, 38.7, 40.9, 42.7, 44.3, 48.8, 50.4, 50.8, 50.4],
      [9.2, 13.4, 19.7, 26.4, 31.9, 35.2, 38.3, 40.2, 41.9, 43.1, 47.8, 49.9, 50.7, 50.3],
      [11.2, 15.3, 21.0, 27.6, 32.7, 35.3, 38.4, 40.8, 42.6, 44.5, 48.6, 50.5, 50.9, 51.0],
      [12.9, 17.1, 22.6, 28.8, 33.5, 36.0, 38.8, 41.0, 43.0, 44.6, 48.7, 50.6, 51.1, 51.0],
      [14.6, 18.7, 24.6, 30.1, 34.2, 36.9, 39.3, 41.3, 43.2, 44.9, 48.9, 51.0, 51.3, 51.5],
      [16.5, 20.9, 26.3, 31.3, 35.0, 37.6, 40.0, 42.0, 43.7, 45.3, 48.8, 50.9, 51.4, 51.7],
      [20.9, 25.3, 30.0, 34.0, 37.0, 39.5, 41.9, 43.4, 45.0, 46.4, 49.3, 51.0, 51.3, 51.9],
      [25.1, 28.9, 33.2, 36.2, 39.2, 41.2, 43.2, 44.7, 46.0, 47.3, 49.6, 51.0, 51.3, 51.6],
      [34.0, 36.6, 39.2, 41.1, 43.2, 44.5, 46.1, 47.2, 48.0, 49.0, 50.3, 51.3, 51.3, 51.2],
      [40.3, 41.8, 43.6, 44.9, 46.1, 47.1, 48.2, 49.2, 49.9, 50.5, 51.2, 51.3, 50.9, 50.7],
      [44.0, 44.8, 46.0, 47.1, 48.4, 49.1, 49.9, 50.7, 51.4, 51.9, 51.6, 51.4, 50.6, 50.2],
      [49.6, 49.7, 50.4, 51.2, 51.8, 52.2, 52.6, 52.9, 53.3, 53.8, 52.6, 51.7, 50.4, 49.6],
      [53.9, 53.7, 54.0, 54.2, 54.4, 54.5, 54.5, 54.4, 54.4, 54.9, 53.1, 51.8, 50.1, 49.1],
      [54.0, 53.7, 53.8, 53.7, 53.5, 53.6, 53.5, 53.3, 53.5, 53.7, 51.4, 49.8, 47.9, 46.6],
      [52.8, 52.4, 52.6, 52.3, 52.2, 52.3, 52.0, 51.9, 51.8, 51.8, 49.5, 47.4, 45.4, 43.8],
      [51.4, 51.2, 51.3, 51.0, 50.8, 50.7, 50.3, 49.9, 49.8, 49.7, 47.6, 45.3, 43.1, 41.4],
      [49.6, 49.6, 49.7, 49.5, 49.5, 49.2, 48.6, 47.9, 47.4, 47.1, 45.1, 42.9, 40.8, 39.2]
  ]

  swapTenors = [ql.Period(tenor) for tenor in swapTenors]
  optionTenors = [ql.Period(tenor) for tenor in optionTenors]
  normal_vols = [[vol / 10000 for vol in row] for row in normal_vols]

  calendar = ql.TARGET()
  bdc = ql.ModifiedFollowing
  dayCounter = ql.ActualActual()
  swaptionVolMatrix = ql.SwaptionVolatilityMatrix(
      calendar, bdc,
      optionTenors, swapTenors, ql.Matrix(normal_vols),
      dayCounter, False, ql.Normal)

SwaptionVolCube1
****************

SwaptionVolCube2
****************

.. function:: ql.SwaptionVolCube2(atmVolStructure, optionTenors, swapTenors, strikeSpreads, volSpreads, swapIndex, shortSwapIndex, vegaWeightedSmileFit)

.. code-block:: python

  optionTenors = ['1y', '2y', '3y']
  swapTenors = [ '5Y', '10Y']
  strikeSpreads = [ -0.01, 0.0, 0.01]
  volSpreads = [
      [0.5, 0.55, 0.6],
      [0.5, 0.55, 0.6],
      [0.5, 0.55, 0.6],
      [0.5, 0.55, 0.6],
      [0.5, 0.55, 0.6],
      [0.5, 0.55, 0.6],
  ]


  optionTenors = [ql.Period(tenor) for tenor in optionTenors]
  swapTenors = [ql.Period(tenor) for tenor in swapTenors]
  volSpreads = [[ql.QuoteHandle(ql.SimpleQuote(v)) for v in row] for row in volSpreads]

  swapIndexBase = ql.EuriborSwapIsdaFixA(ql.Period(1, ql.Years), e6m_yts, ois_yts)
  shortSwapIndexBase = ql.EuriborSwapIsdaFixA(ql.Period(1, ql.Years), e6m_yts, ois_yts)
  vegaWeightedSmileFit = False

  volCube = ql.SwaptionVolatilityStructureHandle(
      ql.SwaptionVolCube2(
          ql.SwaptionVolatilityStructureHandle(swaptionVolMatrix),
          optionTenors,
          swapTenors,
          strikeSpreads,
          volSpreads,
          swapIndexBase,
          shortSwapIndexBase,
          vegaWeightedSmileFit)
  )
  volCube.enableExtrapolation()

SwaptionVolatilityStructureHandle
*********************************

.. function:: ql.SwaptionVolatilityStructureHandle(swaptionVolStructure)

.. code-block:: python

  swaptionVolHandle = ql.SwaptionVolatilityStructureHandle(swaptionVolMatrix)


RelinkableSwaptionVolatilityStructureHandle
*******************************************

.. function:: ql.RelinkableSwaptionVolatilityStructureHandle()

.. code-block:: python

  handle = ql.RelinkableSwaptionVolatilityStructureHandle()
  handle.linkTo(swaptionVolMatrix)



