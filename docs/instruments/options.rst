Options
#######

Vanilla Options
***************

.. function:: ql.VanillaOption(payoff, europeanExercise)

Payoffs:

- `ql.EuropeanExercise(date)`
- `ql.AmericanExercise(earliestDate, latestDate)`
- `ql.BermudanExercise(dates)`
- `ql.RebatedExercise`

Types:

- `ql.Option.Call`
- `ql.Option.Put`

.. code-block:: python

  strike = 100.0
  maturity= ql.Date(15,6,2025)
  option_type = ql.Option.Call
  payoff = ql.PlainVanillaPayoff(option_type, strike)

  europeanExercise = ql.EuropeanExercise(maturity)
  europeanOption = ql.VanillaOption(payoff, europeanExercise)

  americanExercise = ql.AmericanExercise(ql.Date().todaysDate(), maturity)
  americanOption = ql.VanillaOption(payoff, americanExercise)

  bermudanExercise = ql.BermudanExercise([ql.Date(15,6,2024), ql.Date(15,6,2025)])
  bermudanOption = ql.VanillaOption(payoff, bermudanExercise)


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

Quanto Options
**************