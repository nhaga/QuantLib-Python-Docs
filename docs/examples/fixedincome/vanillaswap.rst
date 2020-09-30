Vanilla Swap
############

.. code-block:: python

  import QuantLib as ql


Create a relinkable yield term structure handle and build a curve


.. code-block:: python

  yts = ql.RelinkableYieldTermStructureHandle()

  instruments = [
      ('depo', '6M', 0.025),
      ('fra', '6M', 0.03),
      ('swap', '1Y', 0.031),
      ('swap', '2Y', 0.032),
      ('swap', '3Y', 0.035)
  ]

  helpers = ql.RateHelperVector()
  index = ql.Euribor6M(yts)
  for instrument, tenor, rate in instruments:
      if instrument == 'depo':
          helpers.append( ql.DepositRateHelper(rate, index) )
      if instrument == 'fra':
          monthsToStart = ql.Period(tenor).length()
          helpers.append( ql.FraRateHelper(rate, monthsToStart, index) )
      if instrument == 'swap':
          swapIndex = ql.EuriborSwapIsdaFixA(ql.Period(tenor))
          helpers.append( ql.SwapRateHelper(rate, swapIndex))
  curve = ql.PiecewiseLogCubicDiscount(2, ql.TARGET(), helpers, ql.ActualActual())


Link the built curve to the relinkable yield term structure handle and build a swap pricing engine

.. code-block:: python

  yts.linkTo(curve)
  engine = ql.DiscountingSwapEngine(yts)


Build a vanilla swap and provide a pricing engine

.. code-block:: python

  tenor = ql.Period('2y')
  fixedRate = 0.05
  forwardStart = ql.Period("2D")

  swap = ql.MakeVanillaSwap(tenor, index, fixedRate, forwardStart, Nominal=100, pricingEngine=engine)

Get the fair rate and NPV

.. code-block:: python

  fairRate = swap.fairRate()
  npv = swap.NPV()