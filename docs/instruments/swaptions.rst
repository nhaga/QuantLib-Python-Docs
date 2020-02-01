Swaptions
*********

**Exercises**

- `ql.EuropeanExercise(start)`
- `ql.AmericanExercise(earliestDate, latestDate)`
- `ql.BermudanExercise(dates)`

**Settlement**

- `ql.Settlement.Cash`
- `ql.Settlement.CollateralizedCashPrice`
- `ql.Settlement.ParYieldCurve`
- `ql.Settlement.Physical`
- `ql.Settlement.PhysicalCleared`
- `ql.Settlement.PhysicalOTC`


Swaption
--------

.. function:: ql.Swaption(swap, exercise, settlement=PhysicalOTC)

.. code-block:: python

  calendar = ql.TARGET()
  today = ql.Date().todaysDate()
  exerciseDate = calendar.advance(today, ql.Period('5y'))
  exercise = ql.EuropeanExercise(exerciseDate)
  swap = ql.MakeVanillaSwap(ql.Period('5y'), ql.Euribor6M(), 0.05, ql.Period('5y'))
  swaption = ql.Swaption(swap, exercise)


Nonstandard Swaption
--------------------


FloatFloatSwaption
------------------
