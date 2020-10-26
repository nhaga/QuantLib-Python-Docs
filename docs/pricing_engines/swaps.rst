Swap Pricing Engines
####################

DiscountingSwapEngine
*********************

.. function:: ql.DiscountingSwapEngine(YieldTermStructure)

.. code-block:: python

    yts = ql.YieldTermStructureHandle(ql.FlatForward(2, ql.TARGET(), 0.5, ql.Actual360()))
    engine = ql.DiscountingSwapEngine(yts)
