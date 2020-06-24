Forwards
********

Forward Rate Agreement
----------------------

.. function:: ql.ForwardRateAgreement(valueDate, maturityDate, position, strikeForward, notional, iborIndex, discountCurve=ql.YieldTermStructureHandle())


.. code-block:: python

  fra = ql.ForwardRateAgreement(
      ql.Date(15,6,2020),
      ql.Date(15,12,2020),
      ql.Position.Long,
      0.01,
      1e6,
      ql.Euribor6M(yts),
      yts
  )

  
FixedRateBondForward
--------------------


.. function:: ql.FixedRateBondForward(valueDate, maturityDate, Position::Type, strike, settlementDays, dayCounter , calendar, businessDayConvention, FixedRateBond, yieldTermStructure=ql.YieldTermStructureHandle(),incomeDiscountCurve=ql.YieldTermStructureHandle())

Position:

- `ql.Position.Long`
- `ql.Position.Short`

.. code-block:: python

  valueDate = ql.Date(24, 6, 2020)
  maturityDate = ql.Date(31, 5, 2032)
  position = ql.Position.Long
  strike = 100
  settlementDays = 2
  dayCounter = ql.Actual360()
  calendar = ql.TARGET()
  businessDayConvention = ql.Following
  bond = ql.FixedRateBond(2, ql.TARGET(), 100.0, ql.Date(31, 5, 2032), ql.Date(30, 5, 2035), ql.Period('1Y'), [0.05], ql.ActualActual())
  bond.setPricingEngine(engine)
  fwd = ql.FixedRateBondForward(
      valueDate, maturityDate, position, strike, settlementDays,
      dayCounter , calendar, businessDayConvention, bond, yts, yts)


