Swaption Pricing Engines
########################


BlackSwaptionEngine
*******************


.. function:: ql.BlackSwaptionEngine(yts, quote)

.. function:: ql.BlackSwaptionEngine(yts, swaptionVolatilityStructure)

.. function:: ql.BlackSwaptionEngine(yts, quote, dayCounter)

.. function:: ql.BlackSwaptionEngine(yts, quote, dayCounter, displacement)

.. code-block:: python

    blackEngine = ql.BlackSwaptionEngine(yts, ql.QuoteHandle(ql.SimpleQuote(0.55)))
    blackEngine = ql.BlackSwaptionEngine(yts, ql.QuoteHandle(ql.SimpleQuote(0.55)), ql.ActualActual())
    blackEngine = ql.BlackSwaptionEngine(yts, ql.QuoteHandle(ql.SimpleQuote(0.55)), ql.ActualActual(), 0.01)



BachelierSwaptionEngine
***********************

.. function:: ql.BachelierSwaptionEngine(yts, quote)

.. function:: ql.BachelierSwaptionEngine(yts, swaptionVolatilityStructure)

.. function:: ql.BachelierSwaptionEngine(yts, quote, dayCounter)

.. code-block:: python

    bachelierEngine = ql.BachelierSwaptionEngine(yts, ql.QuoteHandle(ql.SimpleQuote(0.0055)))
    swaption.setPricingEngine(bachelierEngine)
    swaption.NPV()


FdHullWhiteSwaptionEngine
*************************

.. function:: ql.FdHullWhiteSwaptionEngine(model, range, interval)

.. code-block:: python

    model = ql.HullWhite(yts)
    engine = ql.FdHullWhiteSwaptionEngine(model)
    swaption.setPricingEngine(engine)
    swaption.NPV()


FdG2SwaptionEngine
******************

.. function:: ql.FdG2SwaptionEngine(model)

.. code-block:: python

    model = ql.G2(yts)
    engine = ql.FdG2SwaptionEngine(model)
    swaption.setPricingEngine(engine)
    swaption.NPV()


G2SwaptionEngine
****************

.. function:: ql.G2SwaptionEngine(model, range, interval)

.. code-block:: python

    model = ql.G2(yts)
    g2Engine = ql.G2SwaptionEngine(model, 4, 4)
    swaption.setPricingEngine(g2Engine)
    swaption.NPV()


JamshidianSwaptionEngine
************************

.. function:: ql.JamshidianSwaptionEngine(OneFactorAffineModel)

.. function:: ql.JamshidianSwaptionEngine(OneFactorAffineModel, YieldTermStructure)


.. code-block:: python

    model = ql.HullWhite(yts)
    engine = ql.JamshidianSwaptionEngine(model, yts)
    swaption.setPricingEngine(g2Engine)
    swaption.NPV()


TreeSwaptionEngine
******************

.. function:: ql.TreeSwaptionEngine(ShortRateModel, Size, YieldTermStructure)

.. function:: ql.TreeSwaptionEngine(ShortRateModel, Size)

.. function:: ql.TreeSwaptionEngine(ShortRateModel, TimeGrid, YieldTermStructure)

.. function:: ql.TreeSwaptionEngine(ShortRateModel, TimeGrid)


.. code-block:: python

    model = ql.HullWhite(yts)
    engine = ql.TreeSwaptionEngine(model, 10)
    swaption.setPricingEngine(g2Engine)
    swaption.NPV()
