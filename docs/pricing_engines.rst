###############
Pricing Engines
###############

------

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



------

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


------

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


------


Swap Pricing Engines
####################

DiscountingSwapEngine
*********************

.. function:: ql.DiscountingSwapEngine(YieldTermStructure)

.. code-block:: python

    yts = ql.YieldTermStructureHandle(ql.FlatForward(2, ql.TARGET(), 0.5, ql.Actual360()))
    engine = ql.DiscountingSwapEngine(yts)


------


Credit Pricing Engines
######################

IsdaCdsEngine
*************

MidPointCdsEngine
*****************

IntegralCdsEngine
*****************


------


Option Pricing Engines
########################

AnalyticEuropeanEngine
**********************

.. function:: ql.AnalyticEuropeanEngine(GeneralizedBlackScholesProcess)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
    process = ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volatility)

    engine = ql.AnalyticEuropeanEngine(process)


MCEuropeanEngine
****************

.. function:: ql.MCEuropeanEngine(GeneralizedBlackScholesProcess, traits, timeSteps=None, timeStepsPerYear=None, brownianBridge=False, antitheticVariate=False, requiredSamples=None, requiredTolerance=None, maxSamples=None, seed=0)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
    process = ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volatility)

    steps = 2
    rng = "pseudorandom" # could use "lowdiscrepancy"
    numPaths = 100000

    engine = ql.MCEuropeanEngine(process, rng, steps, requiredSamples=numPaths)


FdBlackScholesVanillaEngine
***************************

Note that this engine is capable of pricing both European and American payoffs!

.. function:: ql.FdBlackScholesVanillaEngine(GeneralizedBlackScholesProcess, tGrid, xGrid, dampingSteps=0, schemeDesc=FdmSchemeDesc::Douglas(), localVol=False, illegalLocalVolOverwrite=None)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
    process = ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volatility)

    tGrid, xGrid = 2000, 200
    engine = ql.FdBlackScholesVanillaEngine(process, tGrid, xGrid)


MCAmericanEngine
****************

.. function:: ql.MCAmericanEngine(GeneralizedBlackScholesProcess, traits, timeSteps=None, timeStepsPerYear=None, antitheticVariate=False, controlVariate=False, requiredSamples=None, requiredTolerance=None, maxSamples=None, seed=0, polynomOrder=2, polynomType=0, nCalibrationSamples=2048, antitheticVariateCalibration=None, seedCalibration=None)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
    process = ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volatility)

    steps = 200
    rng = "pseudorandom" # could use "lowdiscrepancy"
    numPaths = 100000

    engine = ql.MCAmericanEngine(process, rng, steps, requiredSamples=numPaths)


AnalyticDiscreteGeometricAveragePriceAsianEngine
************************************************

.. function:: ql.AnalyticDiscreteGeometricAveragePriceAsianEngine(GeneralizedBlackScholesProcess)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
    process = ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volatility)

    engine = ql.AnalyticDiscreteGeometricAveragePriceAsianEngine(process)


AnalyticContinuousGeometricAveragePriceAsianEngine
**************************************************

.. function:: ql.AnalyticContinuousGeometricAveragePriceAsianEngine(GeneralizedBlackScholesProcess)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
    process = ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volatility)

    engine = ql.AnalyticContinuousGeometricAveragePriceAsianEngine(process)


MCDiscreteGeometricAPEngine
***************************

.. function:: ql.MCDiscreteGeometricAPEngine(GeneralizedBlackScholesProcess, traits, brownianBridge=False, antitheticVariate=False, requiredSamples=None, requiredTolerance=None, maxSamples=None, seed=0)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
    process = ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volatility)

    rng = "pseudorandom" # could use "lowdiscrepancy"
    numPaths = 100000

    engine = ql.MCDiscreteGeometricAPEngine(process, rng, requiredSamples=numPaths)


MCDiscreteArithmeticAPEngine
****************************

.. function:: ql.MCDiscreteArithmeticAPEngine(GeneralizedBlackScholesProcess, traits, brownianBridge=False, antitheticVariate=False, controlVariate=False, requiredSamples=None, requiredTolerance=None, maxSamples=None, seed=0)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
    process = ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volatility)

    rng = "pseudorandom" # could use "lowdiscrepancy"
    numPaths = 100000

    engine = ql.MCDiscreteArithmeticAPEngine(process, rng, requiredSamples=numPaths)


FdBlackScholesAsianEngine
*************************

Note that this engine will throw an error if asked to price Geometric averaging options. It only prices Dsicrete Arithmetic Asians.

.. function:: ql.FdBlackScholesAsianEngine(GeneralizedBlackScholesProcess, tGrid=100, xGrid=100, aGrid=50)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
    process = ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volatility)

    tGrid, xGrid, aGrid = 100, 100, 50
    engine = ql.FdBlackScholesAsianEngine(process, tGrid=tGrid, xGrid=xGrid, aGrid=aGrid)


MCEuropeanBasketEngine
**********************

.. function:: ql.MCEuropeanBasketEngine(GeneralizedBlackScholesProcess, traits, timeSteps=None, timeStepsPerYear=None, brownianBridge=False, antitheticVariate=False, requiredSamples=None, requiredTolerance=None, maxSamples=None, seed=0)

.. code-block:: python

  # Create a StochasticProcessArray for the various underlyings
  underlying_spots = [100., 100., 100., 100., 100.]
  underlying_vols = [0.1, 0.12, 0.13, 0.09, 0.11]
  underlying_corr_mat = [[1, 0.1, -0.1, 0, 0], [0.1, 1, 0, 0, 0.2], [-0.1, 0, 1, 0, 0], [0, 0, 0, 1, 0.15], [0, 0.2, 0, 0.15, 1]]

  today = ql.Date().todaysDate()
  day_count = ql.Actual365Fixed()
  calendar = ql.NullCalendar()

  riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.0, day_count))
  dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.0, day_count))

  processes = [ql.BlackScholesMertonProcess(ql.QuoteHandle(ql.SimpleQuote(x)),
                                            dividendTS,
                                            riskFreeTS,
                                            ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, calendar, y, day_count)))
               for x, y in zip(underlying_spots, underlying_vols)]

  multiProcess = ql.StochasticProcessArray(processes, underlying_corr_mat)

  # Create the pricing engine
  rng = "pseudorandom"
  numSteps = 500000
  stepsPerYear = 1
  seed = 43

  engine = ql.MCEuropeanBasketEngine(multiProcess, rng, timeStepsPerYear=stepsPerYear, requiredSamples=numSteps, seed=seed)


AnalyticHestonEngine
********************

.. function:: ql.AnalyticHestonEngine(HestonModel)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))

    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
    v0 = 0.005
    kappa = 0.8
    theta = 0.008
    rho = 0.2
    sigma = 0.1

    hestonProcess = ql.HestonProcess(riskFreeTS, dividendTS, initialValue, v0, kappa, theta, sigma, rho)
    hestonModel = ql.HestonModel(hestonProcess)

    engine = ql.AnalyticHestonEngine(hestonModel)


MCEuropeanHestonEngine
**********************

.. function:: ql.MCEuropeanHestonEngine(HestonProcess, traits, timeSteps=None, timeStepsPerYear=None, antitheticVariate=False, requiredSamples=None, requiredTolerance=None, maxSamples=None, seed=0)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))

    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
    v0 = 0.005
    kappa = 0.8
    theta = 0.008
    rho = 0.2
    sigma = 0.1

    hestonProcess = ql.HestonProcess(riskFreeTS, dividendTS, initialValue, v0, kappa, theta, sigma, rho)

    steps = 2
    rng = "pseudorandom" # could use "lowdiscrepancy"
    numPaths = 100000

    engine = ql.MCEuropeanHestonEngine(hestonProcess, rng, steps, requiredSamples=numPaths)


FdHestonVanillaEngine
*********************

.. function:: ql.FdHestonVanillaEngine(HestonModel, tGrid=100, xGrid=100, vGrid=50, dampingSteps=0, FdmSchemeDesc==FdmSchemeDesc::Hundsdorfer(), leverageFct=LocalVolTermStructure())

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))

    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
    v0 = 0.005
    kappa = 0.8
    theta = 0.008
    rho = 0.2
    sigma = 0.1

    hestonProcess = ql.HestonProcess(riskFreeTS, dividendTS, initialValue, v0, kappa, theta, sigma, rho)
    hestonModel = ql.HestonModel(hestonProcess)

    tGrid, xGrid, vGrid = 100, 100, 50
    dampingSteps = 0
    fdScheme = ql.FdmSchemeDesc.ModifiedCraigSneyd()

    engine = ql.FdHestonVanillaEngine(hestonModel, tGrid, xGrid, vGrid, dampingSteps, fdScheme)

