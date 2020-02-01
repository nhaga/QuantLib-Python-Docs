Caps & Floors
*************

Cap
---

.. function:: ql.Cap(floatingLeg, exerciseRates)

.. code-block:: python

  schedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(16,6,2022), ql.Period('6M'))
  ibor_leg = ql.IborLeg([100], schedule, ql.Euribor6M())
  strike = 0.01
  cap = ql.Cap(ibor_leg, [strike])


Floor
-----

.. function:: ql.Floor(floatingLeg, exerciseRates)

.. code-block:: python

  schedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(16,6,2022), ql.Period('6M'))
  ibor_leg = ql.IborLeg([100], schedule, ql.Euribor6M())
  strike = 0.00
  floor = ql.Floor(ibor_leg, [strike])

Collar
------

.. function:: ql.Collar(floatingLeg, capRates, floorRates)

.. code-block:: python

  schedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(16,6,2022), ql.Period('6M'))
  ibor_leg = ql.IborLeg([100], schedule, ql.Euribor6M())
  capStrike = 0.02
  floorStrike = 0.00
  collar = ql.Collar(ibor_leg, [capStrike], [floorStrike])