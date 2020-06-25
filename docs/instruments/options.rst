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
 
Barrier Options
***************

Basket Options
**************

Cliquet Options
***************

Forward Options
***************

Quanto Options
**************