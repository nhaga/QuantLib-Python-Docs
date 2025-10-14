.. _termstructures-volatility:

Volatility
##########

.. _ql.SmileSection:

SmileSections
*************

A SmileSection in QuantLib is, as the word is saying, a class representing the portion of a volatility surface for a specific tenor.
As we know, the volatility in real life is not flat across different tenors and different strikes, thus a vol surface can be described by a bidimensional function :math:`\sigma: (K, \tau)` that maps a strike and a tenor to a specific volatility.
A smile section, indeed is a function that maps a specific strike to a volatility value :math:`\sigma: K \rightarrow \sigma(K)`, think a partial application of the vol-surface function where the tenor is fixed.

The base class the represent a smile section in QuantLib is the ``SmileSection`` class


.. class:: SmileSection()

   Abstract base class representing a volatility smile at a fixed exercise date.

   A :class:`SmileSection` provides access to the volatility (or variance) 
   surface as a function of **strike**, holding **expiry** constant. 
   It is commonly used in local-volatility calibration, volatility interpolation, 
   and model validation.

    .. note::
      This is an abstract interface. Concrete implementations define the specific 
      functional form of the smile (e.g., :class:`ql.InterpolatedSmileSection`, 
      :class:`ql.SabrSmileSection`, etc.).


    .. method:: minStrike()

      :return: Returns the minimum strike value supported by the smile section.
      :rtype: float

    .. method:: maxStrike()

      :return: Returns the maximum strike value supported by the smile section.
      :rtype: float

    .. method:: atmLevel()

      Returns the at-the-money (ATM) level used within this smile section, 
      typically corresponding to the forward or spot level at expiry.

      :return: The ATM level used in this smile section.
      :rtype: float

    .. method:: variance(strike: float)

      Returns the **total variance** associated with the given strike.

      :param strike: Strike rate at which to evaluate the variance.
      :return: Total variance at the given strike.
      :rtype: float

    .. method:: volatility(strike: float)

      Returns the **volatility** corresponding to the given strike.

      :param strike: Strike rate.
      :return: Volatility at the given strike.
      :rtype: float

    .. method:: volatility(strike: float, type: ql.VolatilityType, shift: float = 0.0)

      Returns the volatility corresponding to the given strike, expressed in a specific 
      volatility type (e.g., normal, lognormal, shifted-lognormal).

      :param strike: Strike rate.
      :param type: The volatility type (see :class:`ql.VolatilityType`).
      :param shift: Optional shift parameter (for shifted models).
      :return: Volatility value.
      :rtype: float

    .. method:: exerciseDate()

      Returns the exercise (expiry) date associated with this smile section.

      :return: The exercise date.
      :rtype: ql.Date

    .. method:: referenceDate()

      Returns the reference (valuation) date used for this smile section.

      :return: The reference date.
      :rtype: ql.Date

    .. method:: exerciseTime()

      Returns the exercise time (in year fractions) corresponding to the expiry.

      :return: Exercise time in year fractions.
      :rtype: float

    .. method:: dayCounter()

      Returns the day-count convention used to compute the exercise time.

      :return: Day-count convention.
      :rtype: ql.DayCounter

    .. method:: volatilityType()

      Returns the volatility type (e.g., lognormal or normal) represented by this smile section.

      :return: Volatility type.
      :rtype: ql.VolatilityType

    .. method:: shift()

      Returns the shift value used when the volatility type is shifted-lognormal.

      :return: Shift value.
      :rtype: float

    .. method:: optionPrice(strike: float, type: ql.Option.Type = ql.Option.Call, discount: float = 1.0)

      Computes the undiscounted option price implied by the smile section.

      :param strike: Strike rate.
      :param type: Option type (:data:`ql.Option.Call` or :data:`ql.Option.Put`).
      :param discount: Discount factor applied to the option payoff.
      :return: Option price implied by the smile.
      :rtype: float

    .. method:: digitalOptionPrice(strike: float, type: ql.Option.Type = ql.Option.Call, discount: float = 1.0, gap: float = 1.0e-5)

      Computes the **digital option** price implied by the smile section 
      using a finite-difference approximation.

      :param strike: Strike rate.
      :param type: Option type.
      :param discount: Discount factor applied to the payoff.
      :param gap: Finite-difference gap size for numerical differentiation.
      :return: Digital option price.
      :rtype: float

    .. method:: vega(strike: float, discount: float = 1.0)

      Returns the **vega** (sensitivity of the option price to volatility) 
      at the given strike.

      :param strike: Strike rate.
      :param discount: Discount factor.
      :return: Vega value.
      :rtype: float

    .. method:: density(strike: float, discount: float = 1.0, gap: float = 1.0e-4)

      Returns the **probability density** implied by the smile section 
      at the given strike, derived via numerical differentiation.

      :param strike: Strike rate.
      :param discount: Discount factor.
      :param gap: Finite-difference step size for derivative approximation.
      :return: Probability density value.
      :rtype: float

The concrete SmileSection classes exported in QuantLib Python are the following:

* ``LinearInterpolatedSmileSection``
* ``CubicInterpolatedSmileSection``
* ``MonotonicCubicInterpolatedSmileSection``
* ``SplineCubicInterpolatedSmileSection``

Those classes can be instantiated using one of the following constructors (example for the base class `InterpolatedSmileSection`, the constructor has the same signature also for the other classes):


.. class:: InterpolatedSmileSection(expiryTime: float, strikes: list[float], stdDevHandles: list[QuoteHandle], atmLevel: QuoteHandle, interpolator: Interpolator = Interpolator(), dc: ql.DayCounter = ql.Actual365Fixed(), type: ql.VolatilityType = ql.ShiftedLognormal, shift: float = 0.0)

.. class:: InterpolatedSmileSection(expiryTime: float, strikes: list[float], stdDevHandles: list[float], atmLevel: float, interpolator: Interpolator = Interpolator(), dc: ql.DayCounter = ql.Actual365Fixed(), type: ql.VolatilityType = ql.ShiftedLognormal, shift: float = 0.0)
  :no-index-entry:

.. class:: InterpolatedSmileSection(date: ql.Date, strikes: list[float], stdDevHandles: list[QuoteHandle], atmLevel: QuoteHandle, dc: ql.DayCounter = ql.Actual365Fixed(), interpolator: Interpolator = Interpolator(), type: ql.VolatilityType = ql.ShiftedLognormal, shift: float = 0.0)
  :no-index-entry:

.. class:: InterpolatedSmileSection(date: ql:Date, strikes: list[float], stdDevHandles: list[float], atmLevel: float, dc : ql.DayCounter = ql.Actual365Fixed(), interpolator: Interpolator = Interpolator(), type: ql.VolatilityType = ql.ShiftedLognormal, shift: float = 0.0)
  :no-index-entry:

EquityFX
********

BlackConstantVol
----------------

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
------------------

.. function:: ql.BlackVarianceCurve(referenceDate, expirations, volatilities, dayCounter)

.. code-block:: python

  referenceDate = ql.Date(30, 9, 2013)
  expirations = [ql.Date(20, 12, 2013), ql.Date(17, 1, 2014), ql.Date(21, 3, 2014)]
  volatilities = [.145, .156, .165]
  volatilityCurve = ql.BlackVarianceCurve(referenceDate, expirations, volatilities, ql.Actual360())

BlackVarianceSurface
--------------------

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
---------------------

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
------------------------------

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
---------------------------

.. function:: ql.BlackVolTermStructureHandle(blackVolTermStructure)

.. code-block:: python

  ql.BlackVolTermStructureHandle(constantVol)
  ql.BlackVolTermStructureHandle(volatilityCurve)
  ql.BlackVolTermStructureHandle(volatilitySurface)

RelinkableBlackVolTermStructureHandle
-------------------------------------

.. function:: ql.RelinkableBlackVolTermStructureHandle()

.. function:: ql.RelinkableBlackVolTermStructureHandle(blackVolTermStructure)

.. code-block:: python

  blackTSHandle = ql.RelinkableBlackVolTermStructureHandle(volatilitySurface)

  blackTSHandle = ql.RelinkableBlackVolTermStructureHandle()
  blackTSHandle.linkTo(volatilitySurface)


LocalConstantVol
----------------

.. function:: ql.LocalConstantVol(date, volatility, dayCounter)

.. code-block:: python

  date = ql.Date().todaysDate()
  volatility = 0.2
  dayCounter = ql.Actual360()

  ql.LocalConstantVol(date, volatility, dayCounter)


LocalVolSurface
---------------

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
-----------------------

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
----------------------------

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

Cap Volatility
**************


ConstantOptionletVolatility
---------------------------

floating reference date, floating market data

.. function:: ql.ConstantOptionletVolatility(settlementDays, cal, bdc, volatility (Quote), dc, type=ShiftedLognormal, displacement=0.0)

fixed reference date, floating market data

.. function:: ql.ConstantOptionletVolatility(settlementDate, cal, bdc, volatility (Quote), dc, type=ShiftedLognormal, displacement=0.0)

floating reference date, fixed market data

.. function:: ql.ConstantOptionletVolatility(settlementDays, cal, bdc, volatility (value), dc, type=ShiftedLognormal, displacement=0.0)

fixed reference date, fixed market data

.. function:: ql.ConstantOptionletVolatility(settlementDate, cal, bdc, volatility (value), dc, type=ShiftedLognormal, displacement=0.0)


.. code-block:: python

  settlementDays = 2
  settlementDate = ql.Date().todaysDate()
  cal = ql.TARGET()
  bdc = ql.ModifiedFollowing
  volatility = 0.55
  vol_quote = ql.QuoteHandle(ql.SimpleQuote(volatility))
  dc = ql.Actual365Fixed()

  #floating reference date, floating market data
  c1 = ql.ConstantOptionletVolatility(settlementDays, cal, bdc, vol_quote, dc, ql.Normal)

  #fixed reference date, floating market data
  c2 = ql.ConstantOptionletVolatility(settlementDate, cal, bdc, vol_quote, dc)

  #floating reference date, fixed market data
  c3 = ql.ConstantOptionletVolatility(settlementDays, cal, bdc, volatility, dc)

  #fixed reference date, fixed market data
  c4 = ql.ConstantOptionletVolatility(settlementDate, cal, bdc, volatility, dc)



CapFloorTermVolCurve
--------------------

Cap/floor at-the-money term-volatility vector.


**floating reference date, floating market data**

.. function:: ql.CapFloorTermVolCurve(settlementDays, calendar, bdc, optionTenors, vols (Quotes), dc=Actual365Fixed)

**fixed reference date, floating market data**

.. function:: ql.CapFloorTermVolCurve(settlementDate, calendar, bdc, optionTenors, vols (Quotes), dc=Actual365Fixed)

**fixed reference date, fixed market data**

.. function:: ql.CapFloorTermVolCurve(settlementDate, calendar, bdc, optionTenors, vols (vector), dc=Actual365Fixed)

**floating reference date, fixed market data**

.. function:: ql.CapFloorTermVolCurve(settlementDays, calendar, bdc, optionTenors, vols (vector), dc=Actual365Fixed)


.. code-block:: python

  settlementDate = ql.Date().todaysDate()
  settlementDays = 2
  calendar = ql.TARGET()
  bdc = ql.ModifiedFollowing
  optionTenors  = [ql.Period('1y'), ql.Period('2y'), ql.Period('3y')]
  vols = [0.55, 0.60, 0.65]

  # fixed reference date, fixed market data
  c3 = ql.CapFloorTermVolCurve(settlementDate, calendar, bdc, optionTenors, vols)

  # floating reference date, fixed market data
  c4 = ql.CapFloorTermVolCurve(settlementDays, calendar, bdc, optionTenors, vols)


CapFloorTermVolSurface
----------------------


**floating reference date, floating market data**

.. function:: ql.CapFloorTermVolSurface(settlementDays, calendar, bdc, expiries, strikes, vol_data (Handle), daycount=ql.Actual365Fixed)

**fixed reference date, floating market data**

.. function:: ql.CapFloorTermVolSurface(settlementDate, calendar, bdc, expiries, strikes, vol_data (Handle), daycount=ql.Actual365Fixed)

**fixed reference date, fixed market data**

.. function:: ql.CapFloorTermVolSurface(settlementDate, calendar, bdc, expiries, strikes, vol_data (Matrix), daycount=ql.Actual365Fixed)

**floating reference date, fixed market data**

.. function:: ql.CapFloorTermVolSurface(settlementDays, calendar, bdc, expiries, strikes, vol_data (Matrix), daycount=ql.Actual365Fixed)


.. code-block:: python

  settlementDate = ql.Date().todaysDate()
  settlementDays = 2
  calendar = ql.TARGET()
  bdc = ql.ModifiedFollowing
  expiries  = [ql.Period('9y'), ql.Period('10y'), ql.Period('12y')]
  strikes = [0.015, 0.02, 0.025]

  black_vols = [
      [1.    , 0.792 , 0.6873],
      [0.9301, 0.7401, 0.6403],
      [0.7926, 0.6424, 0.5602]]


  # fixed reference date, fixed market data
  s3 = ql.CapFloorTermVolSurface(settlementDate, calendar, bdc, expiries, strikes, black_vols)

  # floating reference date, fixed market data
  s4 = ql.CapFloorTermVolSurface(settlementDays, calendar, bdc, expiries, strikes, black_vols)


OptionletStripper1
------------------

.. function:: ql.OptionletStripper1(CapFloorTermVolSurface, index, switchStrikes=Null, accuracy=1.0e-6, maxIter=100, discount=YieldTermStructure, type=ShiftedLognormal, displacement=0.0, dontThrow=false)

.. code-block:: python

  index = ql.Euribor6M()
  optionlet_surf = ql.OptionletStripper1(s3, index, type=ql.Normal)


StrippedOptionletAdapter
------------------------

.. function:: ql.StrippedOptionletAdapter(StrippedOptionletBase)

OptionletVolatilityStructureHandle
----------------------------------

.. function:: ql.OptionletVolatilityStructureHandle(OptionletVolatilityStructure)

.. code-block:: python

  ovs_handle = ql.OptionletVolatilityStructureHandle(
      ql.StrippedOptionletAdapter(optionlet_surf)
  )


RelinkableOptionletVolatilityStructureHandle
--------------------------------------------

.. function:: ql.RelinkableOptionletVolatilityStructureHandle()

.. code-block:: python

  ovs_handle = ql.RelinkableOptionletVolatilityStructureHandle()
  ovs_handle.linkTo(ql.StrippedOptionletAdapter(optionlet_surf))


Swaption Volatility
*******************

ConstantSwaptionVolatility
--------------------------

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
------------------------

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
----------------

SwaptionVolCube2
----------------

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
---------------------------------

.. function:: ql.SwaptionVolatilityStructureHandle(swaptionVolStructure)

.. code-block:: python

  swaptionVolHandle = ql.SwaptionVolatilityStructureHandle(swaptionVolMatrix)


RelinkableSwaptionVolatilityStructureHandle
-------------------------------------------

.. function:: ql.RelinkableSwaptionVolatilityStructureHandle()

.. code-block:: python

  handle = ql.RelinkableSwaptionVolatilityStructureHandle()
  handle.linkTo(swaptionVolMatrix)


SABR
****

SabrSmileSection
----------------

.. function:: ql.SabrSmileSection(date, fwd, [alpha, beta, nu, rho], dayCounter, Real)

.. function:: ql.SabrSmileSection(time, fwd, [alpha, beta, nu, rho], dayCounter, Real)

.. code-block:: python

  alpha = 1.63
  beta = 0.6
  nu = 3.3
  rho = 0.00002

  ql.SabrSmileSection(17/365, 120, [alpha, beta, nu, rho])


sabrVolatility
--------------

.. function::  ql.sabrVolatility(strike, forward, expiryTime, alpha, beta, nu, rho)

.. code-block:: python

  alpha = 1.63
  beta = 0.6
  nu = 3.3
  rho = 0.00002
  ql.sabrVolatility(106, 120, 17/365, alpha, beta, nu, rho)


shiftedSabrVolatility
---------------------

.. function:: ql.shiftedSabrVolatility(strike, forward, expiryTime, alpha, beta, nu, rho, shift)

.. code-block:: python

  alpha = 1.63
  beta = 0.6
  nu = 3.3
  rho = 0.00002
  shift = 50

  ql.shiftedSabrVolatility(106, 120, 17/365, alpha, beta, nu, rho, shift)



sabrFlochKennedyVolatility
--------------------------

.. function:: ql.sabrFlochKennedyVolatility(strike, forward, expiryTime, alpha, beta, nu, rho)
  
.. code-block:: python
  
  alpha = 0.01
  beta = 0.01
  nu = 0.01
  rho = 0.01

  ql.sabrFlochKennedyVolatility(0.01,0.01, 5, alpha, beta, nu, rho)
