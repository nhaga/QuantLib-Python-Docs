Bond Pricing Engines
####################

DiscountingBondEngine
*********************

.. function:: ql.DiscountingBondEngine(discountCurve)

.. code-block:: python

    crv = ql.FlatForward(ql.Date().todaysDate(),0.04875825,ql.Actual365Fixed())
    yts = ql.YieldTermStructureHandle(crv)
    engine = ql.DiscountingBondEngine(yts)

BlackCallableFixedRateBondEngine
********************************

.. function:: ql.BlackCallableFixedRateBondEngine(fwdYieldVol, discountCurve)

.. code-block:: python

    crv = ql.FlatForward(ql.Date().todaysDate(),0.04875825,ql.Actual365Fixed())
    yts = ql.YieldTermStructureHandle(crv)
    vol = ql.QuoteHandle(ql.SimpleQuote(0.55))
    engine = ql.BlackCallableFixedRateBondEngine(vol, yts)

TreeCallableFixedRateEngine
***************************

.. function:: ql.TreeCallableFixedRateBondEngine(shortRateModel, size, discountCurve)

.. code-block:: python

    crv = ql.FlatForward(ql.Date().todaysDate(),0.04875825,ql.Actual365Fixed())
    yts = ql.YieldTermStructureHandle(crv)
    model = ql.Vasicek()
    engine = ql.TreeCallableFixedRateBondEngine(model, 10, yts)

.. function:: ql.TreeCallableFixedRateBondEngine(shortRateModel, size)

.. code-block:: python

    model = ql.Vasicek()
    engine = ql.TreeCallableFixedRateBondEngine(model, 10)

.. function:: ql.TreeCallableFixedRateBondEngine(shortRateModel, TimeGrid, discountCurve)

.. code-block:: python

    crv = ql.FlatForward(ql.Date().todaysDate(),0.04875825,ql.Actual365Fixed())
    yts = ql.YieldTermStructureHandle(crv)
    model = ql.Vasicek()
    grid = ql.TimeGrid(5,10)

    engine = ql.TreeCallableFixedRateBondEngine(model, grid, yts)

.. function:: ql.TreeCallableFixedRateBondEngine(shortRateModel, TimeGrid)

.. code-block:: python

    crv = ql.FlatForward(ql.Date().todaysDate(),0.04875825,ql.Actual365Fixed())
    yts = ql.YieldTermStructureHandle(crv)
    model = ql.Vasicek()
    grid = ql.TimeGrid(5,10)

    engine = ql.TreeCallableFixedRateBondEngine(model, grid)
