Option Pricing Engines
########################


Vanilla Options
***************

AnalyticEuropeanEngine
----------------------

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
----------------

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
---------------------------

Note that this engine is capable of pricing both European and American payoffs!

.. function:: ql.FdBlackScholesVanillaEngine(GeneralizedBlackScholesProcess, tGrid, xGrid, dampingSteps=0, schemeDesc=ql.FdmSchemeDesc.Douglas(), localVol=False, illegalLocalVolOverwrite=None)

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
----------------

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


AnalyticHestonEngine
--------------------

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
----------------------

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
---------------------

If a leverage function (and optional mixing factor) is passed in to this function, it prices using the Heston Stochastic Local Vol model

.. function:: ql.FdHestonVanillaEngine(HestonModel, tGrid=100, xGrid=100, vGrid=50, dampingSteps=0, FdmSchemeDesc=ql.FdmSchemeDesc.Hundsdorfer(), leverageFct=LocalVolTermStructure(), mixingFactor=1.0)

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


Asian Options
*************

AnalyticDiscreteGeometricAveragePriceAsianEngine
------------------------------------------------

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
--------------------------------------------------

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
---------------------------

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
----------------------------

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
-------------------------

Note that this engine will throw an error if asked to price Geometric averaging options. It only prices Discrete Arithmetic Asians.

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


AnalyticDiscreteGeometricAveragePriceAsianHestonEngine
------------------------------------------------------

.. function:: ql.AnalyticDiscreteGeometricAveragePriceAsianHestonEngine(HestonProcess)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))

    v0, kappa, theta, rho, sigma = 0.005, 0.8, 0.008, 0.2, 0.1
    hestonProcess = ql.HestonProcess(riskFreeTS, dividendTS, initialValue, v0, kappa, theta, sigma, rho)

    engine = ql.AnalyticDiscreteGeometricAveragePriceAsianHestonEngine(hestonProcess)


AnalyticContinuousGeometricAveragePriceAsianHestonEngine
--------------------------------------------------------

.. function:: ql.AnalyticContinuousGeometricAveragePriceAsianHestonEngine(HestonProcess)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))

    v0, kappa, theta, rho, sigma = 0.005, 0.8, 0.008, 0.2, 0.1
    hestonProcess = ql.HestonProcess(riskFreeTS, dividendTS, initialValue, v0, kappa, theta, sigma, rho)

    engine = ql.AnalyticContinuousGeometricAveragePriceAsianHestonEngine(hestonProcess)


MCDiscreteGeometricAPHestonEngine
---------------------------

.. function:: ql.MCDiscreteGeometricAPHestonEngine(HestonProcess, traits, antitheticVariate=False, requiredSamples=None, requiredTolerance=None, maxSamples=None, seed=0, timeSteps=None, timeStepsPerYear=None)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))

    v0, kappa, theta, rho, sigma = 0.005, 0.8, 0.008, 0.2, 0.1
    hestonProcess = ql.HestonProcess(riskFreeTS, dividendTS, initialValue, v0, kappa, theta, sigma, rho)

    rng = "pseudorandom" # could use "lowdiscrepancy"
    numPaths = 100000

    engine = ql.MCDiscreteGeometricAPHestonEngine(hestonProcess, rng, requiredSamples=numPaths)


MCDiscreteArithmeticAPHestonEngine
----------------------------

.. function:: ql.MCDiscreteArithmeticAPHestonEngine(HestonProcess, traits, antitheticVariate=False, requiredSamples=None, requiredTolerance=None, maxSamples=None, seed=0, timeSteps=None, timeStepsPerYear=None, controlVariate=False)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))

    v0, kappa, theta, rho, sigma = 0.005, 0.8, 0.008, 0.2, 0.1
    hestonProcess = ql.HestonProcess(riskFreeTS, dividendTS, initialValue, v0, kappa, theta, sigma, rho)

    rng = "pseudorandom" # could use "lowdiscrepancy"
    numPaths = 100000

    engine = ql.MCDiscreteArithmeticAPHestonEngine(hestonProcess, rng, requiredSamples=numPaths)


Barrier Options
***************

BinomialBarrierEngine
---------------------

.. function:: ql.BinomialBarrierEngine(process, type, steps)

.. code-block:: python

    today = ql.Date().todaysDate()

    spotHandle = ql.QuoteHandle(ql.SimpleQuote(100))
    flatRateTs = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    flatVolTs = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.UnitedStates(), 0.2, ql.Actual365Fixed()))
    bsm = ql.BlackScholesProcess(spotHandle, flatRateTs, flatVolTs)

    binomialBarrierEngine = ql.BinomialBarrierEngine(bsm, 'crr', 200)


AnalyticBarrierEngine
---------------------

.. function:: ql.AnalyticBarrierEngine(process)

.. code-block:: python

    today = ql.Date().todaysDate()

    spotHandle = ql.QuoteHandle(ql.SimpleQuote(100))
    flatRateTs = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    flatVolTs = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.UnitedStates(), 0.2, ql.Actual365Fixed()))
    bsm = ql.BlackScholesProcess(spotHandle, flatRateTs, flatVolTs)

    analyticBarrierEngine = ql.AnalyticBarrierEngine(bsm)


FdBlackScholesBarrierEngine
---------------------------

.. function:: ql.FdBlackScholesBarrierEngine(process, tGrid=100, xGrid=100, dampingSteps=0, FdmSchemeDesc=ql.FdmSchemeDesc.Douglas(), localVol=False, illegalLocalVolOverwrite=None)

.. code-block:: python

    today = ql.Date().todaysDate()

    spotHandle = ql.QuoteHandle(ql.SimpleQuote(100))
    flatRateTs = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    flatVolTs = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.UnitedStates(), 0.2, ql.Actual365Fixed()))
    bsm = ql.BlackScholesProcess(spotHandle, flatRateTs, flatVolTs)

    fdBarrierEngine = ql.FdBlackScholesBarrierEngine(bsm)


FdBlackScholesRebateEngine
--------------------------

.. function:: ql.FdBlackScholesRebateEngine(process, tGrid=100, xGrid=100, dampingSteps=0, FdmSchemeDesc=ql.FdmSchemeDesc.Douglas(), localVol=False, illegalLocalVolOverwrite=None)

.. code-block:: python

    today = ql.Date().todaysDate()

    spotHandle = ql.QuoteHandle(ql.SimpleQuote(100))
    flatRateTs = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    flatVolTs = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.UnitedStates(), 0.2, ql.Actual365Fixed()))
    bsm = ql.BlackScholesProcess(spotHandle, flatRateTs, flatVolTs)

    fdRebateEngine = ql.FdBlackScholesRebateEngine(bsm)


AnalyticBinaryBarrierEngine
---------------------------

.. function:: ql.AnalyticBinaryBarrierEngine(process)

.. code-block:: python

    today = ql.Date().todaysDate()

    spotHandle = ql.QuoteHandle(ql.SimpleQuote(100))
    flatRateTs = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    flatVolTs = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.UnitedStates(), 0.2, ql.Actual365Fixed()))
    bsm = ql.BlackScholesProcess(spotHandle, flatRateTs, flatVolTs)

    analyticBinaryBarrierEngine = ql.AnalyticBinaryBarrierEngine(bsm)


FdHestonBarrierEngine
---------------------

If a leverage function (and optional mixing factor) is passed in to this function, it prices using the Heston Stochastic Local Vol model

.. function:: ql.FdHestonBarrierEngine(HestonModel, tGrid=100, xGrid=100, vGrid=50, dampingSteps=0, FdmSchemeDesc=ql.FdmSchemeDesc.Hundsdorfer(), leverageFct=LocalVolTermStructure(), mixingFactor=1.0)

.. code-block:: python

    today = ql.Date().todaysDate()

    spotHandle = ql.QuoteHandle(ql.SimpleQuote(100))
    flatRateTs = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    flatDividendTs = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))

    v0, kappa, theta, sigma, rho = 0.01, 2.0, 0.01, 0.01, 0.0
    hestonProcess = ql.HestonProcess(flatRateTs, flatDividendTs, spotHandle, v0, kappa, theta, sigma, rho)
    hestonModel = ql.HestonModel(hestonProcess)

    hestonBarrierEngine = ql.FdHestonBarrierEngine(hestonModel)


AnalyticDoubleBarrierEngine
---------------------------

.. function:: ql.AnalyticDoubleBarrierEngine(process)

.. code-block:: python

    today = ql.Date().todaysDate()

    spotHandle = ql.QuoteHandle(ql.SimpleQuote(100))
    flatRateTs = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    flatVolTs = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.UnitedStates(), 0.2, ql.Actual365Fixed()))
    bsm = ql.BlackScholesProcess(spotHandle, flatRateTs, flatVolTs)

    analyticDoubleBarrierEngine = ql.AnalyticDoubleBarrierEngine(bsm)


AnalyticDoubleBarrierBinaryEngine
---------------------------------

.. function:: ql.AnalyticDoubleBarrierBinaryEngine(process)

.. code-block:: python

    today = ql.Date().todaysDate()

    spotHandle = ql.QuoteHandle(ql.SimpleQuote(100))
    flatRateTs = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    flatVolTs = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.UnitedStates(), 0.2, ql.Actual365Fixed()))
    bsm = ql.BlackScholesProcess(spotHandle, flatRateTs, flatVolTs)

    analyticDoubleBinaryBarrierEngine = ql.AnalyticDoubleBarrierBinaryEngine(bsm)


FdHestonDoubleBarrierEngine
---------------------------

If a leverage function (and optional mixing factor) is passed in to this function, it prices using the Heston Stochastic Local Vol model

.. function:: ql.FdHestonDoubleBarrierEngine(HestonModel, tGrid=100, xGrid=100, vGrid=50, dampingSteps=0, FdmSchemeDesc=ql.FdmSchemeDesc.Hundsdorfer(), leverageFct=LocalVolTermStructure(), mixingFactor=1.0)

.. code-block:: python

    today = ql.Date().todaysDate()

    spotHandle = ql.QuoteHandle(ql.SimpleQuote(100))
    flatRateTs = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    flatDividendTs = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))

    v0, kappa, theta, sigma, rho = 0.01, 2.0, 0.01, 0.01, 0.0
    hestonProcess = ql.HestonProcess(flatRateTs, flatDividendTs, spotHandle, v0, kappa, theta, sigma, rho)
    hestonModel = ql.HestonModel(hestonProcess)

    hestonDoubleBarrierEngine = ql.FdHestonDoubleBarrierEngine(hestonModel)


Basket Options
**************

MCEuropeanBasketEngine
----------------------

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


Cliquet Options
***************

Forward Options
***************


ForwardEuropeanEngine
---------------------

This engine in python implements the C++ engine QuantLib::ForwardVanillaEngine (notice the subtle name change)

.. function:: ql.ForwardEuropeanEngine(process)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
    process = ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volatility)

    engine = ql.ForwardEuropeanEngine(process)


Quanto Options
**************
