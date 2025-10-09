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


MCDigitalEngine
---------------

This engine prices american (ie. knock-in) cash-or-nothing payoffs only

.. function:: ql.MCDigitalEngine(GeneralizedBlackScholesProcess, traits, timeSteps=None, timeStepsPerYear=None, brownianBridge=False, antitheticVariate=False, requiredSamples=None, requiredTolerance=None, maxSamples=None, seed=0)

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

    engine = ql.MCDigitalEngine(process, rng, steps, requiredSamples=numPaths)


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


AnalyticPTDHestonEngine
-----------------------

.. function:: ql.AnalyticPTDHestonEngine(PiecewiseTimeDependentHestonModel)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))

    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))

    times = [1.0, 2.0, 3.0]
    grid = ql.TimeGrid(times)

    v0 = 0.005
    theta = [0.010, 0.015, 0.02]
    kappa = [0.600, 0.500, 0.400]
    sigma = [0.400, 0.350, 0.300]
    rho = [-0.15, -0.10, -0.00]

    kappaTS = ql.PiecewiseConstantParameter(times[:-1], ql.PositiveConstraint())
    thetaTS = ql.PiecewiseConstantParameter(times[:-1], ql.PositiveConstraint())
    rhoTS = ql.PiecewiseConstantParameter(times[:-1], ql.BoundaryConstraint(-1.0, 1.0))
    sigmaTS = ql.PiecewiseConstantParameter(times[:-1], ql.PositiveConstraint())

    for i, time in enumerate(times):
        kappaTS.setParam(i, kappa[i])
        thetaTS.setParam(i, theta[i])
        rhoTS.setParam(i, rho[i])
        sigmaTS.setParam(i, sigma[i])

    hestonModelPTD = ql.PiecewiseTimeDependentHestonModel(riskFreeTS, dividendTS, initialValue, v0, thetaTS, kappaTS, sigmaTS, rhoTS, grid)
    engine = ql.AnalyticPTDHestonEngine(hestonModelPTD)


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
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))

    v0, kappa, theta, rho, sigma = 0.005, 0.8, 0.008, 0.2, 0.1
    hestonProcess = ql.HestonProcess(riskFreeTS, dividendTS, initialValue, v0, kappa, theta, sigma, rho)

    engine = ql.AnalyticContinuousGeometricAveragePriceAsianHestonEngine(hestonProcess)


MCDiscreteGeometricAPHestonEngine
---------------------------------

.. function:: ql.MCDiscreteGeometricAPHestonEngine(HestonProcess, traits, antitheticVariate=False, requiredSamples=None, requiredTolerance=None, maxSamples=None, seed=0, timeSteps=None, timeStepsPerYear=None)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))

    v0, kappa, theta, rho, sigma = 0.005, 0.8, 0.008, 0.2, 0.1
    hestonProcess = ql.HestonProcess(riskFreeTS, dividendTS, initialValue, v0, kappa, theta, sigma, rho)

    rng = "pseudorandom" # could use "lowdiscrepancy"
    numPaths = 100000

    engine = ql.MCDiscreteGeometricAPHestonEngine(hestonProcess, rng, requiredSamples=numPaths)


MCDiscreteArithmeticAPHestonEngine
----------------------------------

.. function:: ql.MCDiscreteArithmeticAPHestonEngine(HestonProcess, traits, antitheticVariate=False, requiredSamples=None, requiredTolerance=None, maxSamples=None, seed=0, timeSteps=None, timeStepsPerYear=None, controlVariate=False)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))

    v0, kappa, theta, rho, sigma = 0.005, 0.8, 0.008, 0.2, 0.1
    hestonProcess = ql.HestonProcess(riskFreeTS, dividendTS, initialValue, v0, kappa, theta, sigma, rho)

    rng = "pseudorandom" # could use "lowdiscrepancy"
    numPaths = 100000

    engine = ql.MCDiscreteArithmeticAPHestonEngine(hestonProcess, rng, requiredSamples=numPaths)


TurnbullWakemanAsianEngine
--------------------------

.. function:: ql.TurnbullWakemanAsianEngine(GeneralizedBlackScholesProcess)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
    process = ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volatility)

    engine = ql.TurnbullWakemanAsianEngine(process)


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


VannaVolgaBarrierEngine
---------------------------

The Vanna–Volga Barrier Engine is a pricing engine for FX exotics options that adjusts the standard Black–Scholes price to account for volatility smiles.

This method uses the Vanna–Volga correction technique, which re-prices barrier options by combining the prices of three liquid market instruments — typically an ATM option, a 25Δ call, and a 25Δ put. These instruments define a volatility smile used to derive a more realistic implied volatility for exotic payoffs.

The engine is specifically made FX options, where implied volatility varies significantly across strikes (the “smile”). By applying smile-adjusted volatilities, the engine provides more market-consistent valuations than flat-volatility models.

For more info see `Castagna A., Mercurio F., 2007. The Vanna-Volga method for implied volatilities <https://www.deriscope.com/docs/The_Vanna_Volga_method_for_implied_volatilities_Castagna_Mercurio_2007.pdf>`_ .

.. class:: ql.VannaVolgaBarrierEngine(atmVol: ql.DeltaVolQuoteHandle, vol25Put: ql.DeltaVolQuoteHandle, vol25Call: ql.DeltaVolQuoteHandle, spotFX: ql.QuoteHandle, domesticTS: ql.YieldTermStructureHandle, foreignTS: ql.YieldTermStructureHandle, adaptVanDelta: bool, bsPriceWithSmile: float)

	VannaVolgaBarrierEngine class constructor

	:param atmVol: The at-the-money volatility quote used as the reference point in the smile construction.  
	:type atmVol: ql.DeltaVolQuoteHandle  

	:param vol25Put: The 25-delta put volatility quote, representing the lower wing of the smile.  
	:type vol25Put: ql.DeltaVolQuoteHandle  

	:param vol25Call: The 25-delta call volatility quote, representing the upper wing of the smile.  
	:type vol25Call: ql.DeltaVolQuoteHandle  

	:param spotFX: The current FX spot rate.  
	:type spotFX: ql.QuoteHandle  

	:param domesticTS: The domestic yield term structure used for discounting domestic currency cash flows.  
	:type domesticTS: ql.YieldTermStructureHandle  

	:param foreignTS: The foreign yield term structure used for discounting foreign currency cash flows.  
	:type foreignTS: ql.YieldTermStructureHandle  

	:param adaptVanDelta: Flag to adjust the Vanna term dynamically to maintain delta neutrality. Defaults to ``False``.  
	:type adaptVanDelta: bool, optional  

	:param bsPriceWithSmile: Scaling factor applied to the Vanna–Volga correction. Default is ``1.0`` (full correction).  
	:type bsPriceWithSmile: float, optional  


.. code-block:: python
    
    spot_quote = ql.SimpleQuote(EUR_USD)
    spot_handle = ql.QuoteHandle(spot_quote)
    
    # Setting up the DeltaVolQuote Handles
    atm_dvol_handle = ql.DeltaVolQuoteHandle(ql.DeltaVolQuote(ql.QuoteHandle(atm_vol_quote), delta_type, maturity_time, atm_type))
    delta25_c_dvol_handle = ql.DeltaVolQuoteHandle(ql.DeltaVolQuote(0.25, ql.QuoteHandle(delta25_call_vol_quote), maturity_time, delta_type))
    delta25_p_dvol_handle = ql.DeltaVolQuoteHandle(ql.DeltaVolQuote(-0.25, ql.QuoteHandle(delta25_put_vol_quote), maturity_time, delta_type))
	
    # Setting up the discount curves
    eur_curve = ql.DiscountCurve(eur_dates, eur_dfs, dc)
    usd_curve = ql.DiscountCurve(usd_dates, usd_dfs, dc)
    
    vanna_volga_ engine = ql.VannaVolgaBarrierEngine(
		atm_dvol_handle, 
		delta25_p_dvol_handle, 
		delta25_c_dvol_handle, 
		spot_handle,
		usd_disc_ts,
		eur_disc_ts
	)

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


AnalyticPartialTimeBarrierOptionEngine
----------------------

.. function:: ql.AnalyticPartialTimeBarrierOptionEngine(process)

.. code-block:: python

    today = ql.Date.todaysDate()
	ql.Settings.instance().evaluationDate = today

    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.makeQuoteHandle(100)
    process = ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volatility)

    engine = ql.AnalyticPartialTimeBarrierOptionEngine(process)


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


MCForwardEuropeanBSEngine
-------------------------

.. function:: ql.MCForwardEuropeanBSEngine(process, traits, timeSteps=None, timeStepsPerYear=None, brownianBridge=False, antitheticVariate=False, requiredSamples=None, requiredTolerance=None, maxSamples=None, seed=0)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    volatility = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), 0.1, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))

    process = ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volatility)

    rng = "pseudorandom" # could use "lowdiscrepancy"
    numPaths = 100000

    engine = ql.MCForwardEuropeanBSEngine(process, rng, timeStepsPerYear=12, requiredSamples=numPaths)


AnalyticHestonForwardEuropeanEngine
-----------------------------------

.. function:: ql.AnalyticHestonForwardEuropeanEngine(process, integrationOrder=144)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))

    v0, kappa, theta, rho, sigma = 0.005, 0.8, 0.008, 0.2, 0.2
    hestonProcess = ql.HestonProcess(riskFreeTS, dividendTS, initialValue, v0, kappa, theta, sigma, rho)

    engine = ql.AnalyticHestonForwardEuropeanEngine(hestonProcess)


MCForwardEuropeanHestonEngine
-----------------------------

.. function:: ql.MCForwardEuropeanHestonEngine(hestonProcess, traits, timeSteps=None, timeStepsPerYear=None, antitheticVariate=False, requiredSamples=None, requiredTolerance=None, maxSamples=None, seed=0, controlVariate=False)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))

    v0, kappa, theta, rho, sigma = 0.005, 0.8, 0.008, 0.2, 0.2
    hestonProcess = ql.HestonProcess(riskFreeTS, dividendTS, initialValue, v0, kappa, theta, sigma, rho)

    rng = "pseudorandom" # could use "lowdiscrepancy"
    numPaths = 100000

    engine = ql.MCForwardEuropeanHestonEngine(hestonProcess, rng, timeStepsPerYear=12, requiredSamples=numPaths)


Quanto Options
**************

FX Options related calculators
*********************


BlackDeltaCalculator
--------------------

A calculator class to calculate the relevant strike for FX-style delta-maturity-vol quotes (see `Reiswich D., Wystup U., 2010. A Guide to FX Options Quoting Conventions <https://www.researchgate.net/publication/275905055_A_Guide_to_FX_Options_Quoting_Conventions>`_) 

.. class:: BlackDeltaCalculator(optionType: ql.Option, deltaType: ql.DeltaVolQuote, spot: float, dDiscount: float, fDiscount: float, stdDev: float)

	BlackDeltaCalculator class constructor

	:param optionType: Option type (call or put)
	:type optionType: ql.Option
	:param deltaType: Delta type (spot, forward, premium-adjusted, etc.)
	:type deltaType: :py:class:`ql.DeltaVolQuote`
	:param spot: Spot FX rate
	:type spot: float
	:param dDiscount: Domestic discount factor
	:type dDiscount: float
	:param fDiscount: Foreign discount factor
	:type fDiscount: float
	:param stdDev: Standard deviation of the underlying (volatility * sqrt(timeToMaturity))
	:type stdDev: float

	.. warning::
		Instead of volatility, this parameter uses standard deviation, i.e. volatility * sqrt(timeToMaturity).
		

.. method:: deltaFromStrike(strike: float)

    Computes the option delta for a given strike using the Black-Scholes formula and the delta convention specified at construction (spot, forward, premium-adjusted, etc.).

    :param strike: The option strike price.
    :type strike: float
    :return: The option delta under the chosen convention.
    :rtype: float

.. method:: strikeFromDelta(delta: float)

    Computes the strike corresponding to a given delta by inverting the Black-Scholes formula, according to the delta convention set at construction. Useful for constructing volatility smiles and quoting FX options by delta.

    :param delta: The target option delta (under the chosen convention).
    :type delta: float
    :return: The strike price corresponding to the given delta.
    :rtype: float

.. method:: atmStrike(atmType: ql.DeltaVolQuote)

    Calculates the at-the-money (ATM) strike for the given ATM convention. Determines the strike price that corresponds to "at-the-money" under different conventions commonly used in FX markets.

    :param atmType: The ATM convention to use.
    :type atmType: ql.DeltaVolQuote
    :return: The ATM strike price according to the specified convention.
    :rtype: float

    .. note::
        This calculation is independent of the strike and uses the forward rate, volatility, and time to expiration set at construction.

Possible values of `DeltaVolQuote.AtmType`:

* ``AtmNull``: No ATM convention (returns null)
* ``AtmSpot``: ATM strike equals the current spot rate
* ``AtmForward``: ATM strike equals the forward rate
* ``AtmDeltaNeutral``: ATM strike where call and put deltas sum to zero
* ``AtmVegaMax``: ATM strike that maximizes vega (typically close to forward)
* ``AtmGammaMax``: ATM strike that maximizes gamma
* ``AtmPutCall25``: ATM strike where 25-delta call and put have equal volatility

.. method:: setDeltaType(deltaType: ql.DeltaVolQuote)

    Sets the delta calculation convention.

    :param deltaType: The new delta type convention.
    :type deltaType: ql.DeltaVolQuote

.. method:: setOptionType(optionType: ql.Option)

    Sets the option type (call or put).

    :param optionType: The option type.
    :type optionType: ql.Option
	
**Examples:**

.. code-block:: python

    import numpy as np

    today = ql.Date().todaysDate()
    dayCounter = ql.Actual365Fixed()
    spot = 100
    rd, rf = 0.02, 0.05

    ratesTs = ql.YieldTermStructureHandle(ql.FlatForward(today, rd, dayCounter))
    dividendTs = ql.YieldTermStructureHandle(ql.FlatForward(today, rf, dayCounter))

    # Details about the delta quote
    optionType = ql.Option.Put
    vol = 0.07
    maturity = 1.0
    deltaType = ql.DeltaVolQuote.Fwd      # Also supports: Spot, PaSpot, PaFwd

    # Set up the calculator
    localDcf, foreignDcf = ratesTs.discount(maturity), dividendTs.discount(maturity)
    stdDev = np.sqrt(maturity) * vol
    calc = ql.BlackDeltaCalculator(optionType, deltaType, spot, localDcf, foreignDcf, stdDev)


To calculate the strike for a given call/put delta (negative for put delta)

.. code-block:: python

    delta = -0.3
    calc.strikeFromDelta(delta)


Or if this is an ATM quote, specify the ATM convention

.. code-block:: python

    atmType = ql.DeltaVolQuote.AtmFwd     # Also supports: AtmSpot, AtmDeltaNeutral, AtmVegaMax, AtmGammaMax, AtmPutCall50
    calc.atmStrike(atmType)
