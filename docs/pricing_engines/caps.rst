Cap Pricing Engines
###################

BlackCapFloorEngine
*******************


.. function:: ql.BlackCapFloorEngine(yieldTermStructure, quoteHandle)


.. code-block:: python

    vols = ql.QuoteHandle(ql.SimpleQuote(0.547295))
    engine = ql.BlackCapFloorEngine(yts, vols)
    cap.setPricingEngine(engine)

.. function:: ql.BlackCapFloorEngine(yieldTermStructure, OptionletVolatilityStructure)


BachelierCapFloorEngine
***********************

.. function:: ql.BachelierCapFloorEngine(yieldTermStructure, quoteHandle)

.. code-block:: python

    vols = ql.QuoteHandle(ql.SimpleQuote(0.00547295))
    engine = ql.BachelierCapFloorEngine(yts, vols)

.. function:: ql.BachelierCapFloorEngine(yieldTermStructure, OptionletVolatilityStructure)


AnalyticCapFloorEngine
**********************

.. function:: ql.AnalyticCapFloorEngine(OneFactorAffineModel, YieldTermStructure)

.. function:: ql.AnalyticCapFloorEngine(OneFactorAffineModel)

OneFactorAffineModel

- HullWhite : (termStructure, a=0.1, sigma=0.01)
- Vasicek : (r0=0.05, a=0.1, b=0.05, sigma=0.01, lambda=0.0)
- CoxIngersollRoss [NOT IMPLEMENTED]
- GeneralizedHullWhite [NOT IMPLEMENTED]

.. code-block:: python

    yts = ql.YieldTermStructureHandle(ql.FlatForward(ql.Date().todaysDate(), 0.0121, ql.Actual360()))
    models = [
        ql.HullWhite (yts),
        ql.Vasicek(r0=0.008),
    ]

    for model in models:
        analyticEngine = ql.AnalyticCapFloorEngine(model, yts)
        cap.setPricingEngine(analyticEngine)
        print(f"Cap npv is: {cap.NPV():,.2f}")


TreeCapFloorEngine
******************


.. function:: ql.TreeCapFloorEngine(ShortRateModel, Size, YieldTermStructure)

.. function:: ql.TreeCapFloorEngine(ShortRateModel, Size)

.. function:: ql.TreeCapFloorEngine(ShortRateModel, Size, TimeGrid, YieldTermStructure)

.. function:: ql.TreeCapFloorEngine(ShortRateModel, Size, TimeGrid)

Models

- HullWhite : (YieldTermStructure, a=0.1, sigma=0.01)
- BlackKarasinski : (YieldTermStructure, a=0.1, sigma=0.1)
- Vasicek : (r0=0.05, a=0.1, b=0.05, sigma=0.01, lambda=0.0)
- G2 : (termStructure, a=0.1, sigma=0.01, b=0.1, eta=0.01, rho=-0.75)
- GeneralizedHullWhite [NOT IMPLEMENTED]
- CoxIngersollRoss [NOT IMPLEMENTED]
- ExtendedCoxIngersollRoss [NOT IMPLEMENTED]

.. code-block:: python

    models = [
        ql.HullWhite (yts),
        ql.BlackKarasinski(yts),
        ql.Vasicek(0.0065560),
        ql.G2(yts)
    ]

    for model in models:
        treeEngine = ql.TreeCapFloorEngine(model, 60, yts)
        cap.setPricingEngine(treeEngine)
        print(f"Cap npv is: {cap.NPV():,.2f}")
