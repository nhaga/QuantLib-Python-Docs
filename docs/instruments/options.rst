Options
#######

Vanilla Options
***************

.. function:: ql.VanillaOption(payoff, europeanExercise)

Exercise Types:

- `ql.EuropeanExercise(date)`
- `ql.AmericanExercise(earliestDate, latestDate)`
- `ql.BermudanExercise(dates)`
- `ql.RebatedExercise`

Payoffs:

- `ql.Option.Call`
- `ql.Option.Put`

.. code-block:: python

  strike = 100.0
  maturity = ql.Date(15,6,2025)
  option_type = ql.Option.Call

  payoff = ql.PlainVanillaPayoff(option_type, strike)
  binaryPayoff = ql.CashOrNothingPayoff(option_type, strike, 1)

  europeanExercise = ql.EuropeanExercise(maturity)
  europeanOption = ql.VanillaOption(payoff, europeanExercise)

  americanExercise = ql.AmericanExercise(ql.Date().todaysDate(), maturity)
  americanOption = ql.VanillaOption(payoff, americanExercise)

  bermudanExercise = ql.BermudanExercise([ql.Date(15,6,2024), ql.Date(15,6,2025)])
  bermudanOption = ql.VanillaOption(payoff, bermudanExercise)

  binaryOption = ql.VanillaOption(binaryPayoff, european_exercise)


Asian Options
*************

.. function:: ql.DiscreteAveragingAsianOption(averageType, runningAccumulator, pastFixings, fixingDates, payoff, exercise)

Averaging Types:

- `ql.ContinuousAveragingAsianOption(arithmeticAverage, vanillaPayoff, europeanExercise)`
- `ql.DiscreteAveragingAsianOption(arithmeticAverage, arithmeticRunningAccumulator, pastFixings, asianFutureFixingDates, vanillaPayoff, europeanExercise)`

Average Definitions:

- `ql.Average().Arithmetic`
- `ql.Average().Geometric`

.. code-block:: python

  today = ql.Date().todaysDate()
  periods = [ql.Period("6M"), ql.Period("12M"), ql.Period("18M"), ql.Period("24M")]

  pastFixings = 0 # Empty because this is a new contract
  asianFutureFixingDates = [today + period for period in periods]
  asianExpiryDate = today + periods[-1]

  strike = 100
  vanillaPayoff = ql.PlainVanillaPayoff(ql.Option.Call, strike)
  europeanExercise = ql.EuropeanExercise(asianExpiryDate)

  arithmeticAverage = ql.Average().Arithmetic
  arithmeticRunningAccumulator = 0.0
  discreteArithmeticAsianOption = ql.DiscreteAveragingAsianOption(arithmeticAverage, arithmeticRunningAccumulator, pastFixings, asianFutureFixingDates, vanillaPayoff, europeanExercise)

  geometricAverage = ql.Average().Geometric
  geometricRunningAccumulator = 1.0
  discreteGeometricAsianOption = ql.DiscreteAveragingAsianOption(geometricAverage, geometricRunningAccumulator, pastFixings, asianFutureFixingDates, vanillaPayoff, europeanExercise)

  continuousGeometricAsianOption = ql.ContinuousAveragingAsianOption(geometricAverage, vanillaPayoff, europeanExercise)

 
Barrier Options
***************

.. function:: ql.BarrierOption(barrierType, barrier, rebate, payoff, exercise)

Barrier Types:

- `ql.Barrier.UpIn`
- `ql.Barrier.UpOut`
- `ql.Barrier.DownIn`
- `ql.Barrier.DownOut`

.. code-block:: python

  T = 1
  K = 100.
  barrier = 110.
  rebate = 0.
  barrierType = ql.Barrier.UpOut

  today = ql.Date().todaysDate()
  maturity = today + ql.Period(int(T*365), ql.Days)

  payoff = ql.PlainVanillaPayoff(ql.Option.Call, K)
  amExercise = ql.AmericanExercise(today, maturity, True)
  euExercise = ql.EuropeanExercise(maturity)

  barrierOption = ql.BarrierOption(barrierType, barrier, rebate, payoff, euExercise)


.. function:: ql.DoubleBarrierOption(barrierType, barrier_lo, barrier_hi, rebate, payoff, exercise)

Double Barrier Types:

- `ql.DoubleBarrier.KnockIn`
- `ql.DoubleBarrier.KnockOut`
- `ql.DoubleBarrier.KIKO`
- `ql.DoubleBarrier.KOKI`

.. code-block:: python

  T = 1
  K = 100.
  barrier_lo, barrier_hi = 90., 110.
  rebate = 0.
  barrierType = ql.DoubleBarrier.KnockOut

  today = ql.Date().todaysDate()
  maturity = today + ql.Period(int(T*365), ql.Days)

  payoff = ql.PlainVanillaPayoff(ql.Option.Call, K)
  euExercise = ql.EuropeanExercise(maturity)

  doubleBarrierOption = ql.DoubleBarrierOption(barrierType, barrier_lo, barrier_hi, rebate, payoff, euExercise)


Basket Options
**************

.. function:: ql.BasketOption(payoff, exercise)

Payoff Types:

- `ql.MinBasketPayoff(payoff)`
- `ql.AverageBasketPayoff(payoff, numInstruments)`
- `ql.MaxBasketPayoff(payoff)`

.. code-block:: python

  today = ql.Date().todaysDate()
  exp_date = today + ql.Period(1, ql.Years)
  strike = 100
  number_of_underlyings = 5

  exercise = ql.EuropeanExercise(exp_date)
  vanillaPayoff = ql.PlainVanillaPayoff(ql.Option.Call, strike)

  payoffMin = ql.MinBasketPayoff(vanillaPayoff)
  basketOptionMin = ql.BasketOption(payoffMin, exercise)

  payoffAverage = ql.AverageBasketPayoff(vanillaPayoff, number_of_underlyings)
  basketOptionAverage = ql.BasketOption(payoffAverage, exercise)

  payoffMax = ql.MaxBasketPayoff(vanillaPayoff)
  basketOptionMax = ql.BasketOption(payoffMax, exercise)


Cliquet Options
***************

Forward Options
***************

.. function:: ql.ForwardVanillaOption(moneyness, resetDate, payoff, exercise)

.. code-block:: python

  today = ql.Date().todaysDate()
  resetDate = today + ql.Period(1, ql.Years)
  expiryDate = today + ql.Period(2, ql.Years)
  moneyness, strike = 1., 100 # nb. strike is required for the payoff, but ignored in pricing

  exercise = ql.EuropeanExercise(expiryDate)
  vanillaPayoff = ql.PlainVanillaPayoff(ql.Option.Call, strike)

  forwardStartOption = ql.ForwardVanillaOption(moneyness, resetDate, vanillaPayoff, exercise)


Quanto Options
**************