Credit
######

CreditDefaultSwap
*****************

.. function:: ql.CreditDefaultSwap(side, nominal, spread, cdsSchedule, convention, dayCounter)

.. code-block:: python

  side = ql.Protection.Seller
  nominal = 10e6
  spread = 34.6 / 10000
  cdsSchedule = ql.MakeSchedule(ql.Date(20, 12, 2019), ql.Date(20, 12, 2024), ql.Period('3M'),
                              ql.Quarterly, ql.TARGET(), ql.Following, ql.Unadjusted, ql.DateGeneration.TwentiethIMM)

  cds = ql.CreditDefaultSwap(side, nominal, spread, cdsSchedule, ql.Following, ql.Actual360())

CdsOption
*********

.. function:: ql.CdsOption(CreditDefaultSwap, exercise, knocksOut=true)

..code-block:: python

  expiry = ql.Date(15,6,2020)
  exercise = ql.EuropeanExercise(expiry)
  ql.CdsOption(cds, exercise, True)

