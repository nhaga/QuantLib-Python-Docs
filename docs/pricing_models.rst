##############
Pricing Models
##############


Equity
######

Heston
******

HestonModel
-----------

.. function:: ql.HestonModel(HestonProcess)

.. code-block:: python

    today = ql.Date().todaysDate()
    riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
    dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))

    initialValue = ql.QuoteHandle(ql.SimpleQuote(100))

    v0 = 0.005
    theta = 0.010
    kappa = 0.600
    sigma = 0.400
    rho = -0.15

    hestonProcess = ql.HestonProcess(riskFreeTS, dividendTS, initialValue, v0, kappa, theta, sigma, rho)
    hestonModel = ql.HestonModel(hestonProcess)


PiecewiseTimeDependentHestonModel
---------------------------------

.. function:: ql.PiecewiseTimeDependentHestonModel(riskFreeRate, dividendYield, s0, v0, theta, kappa, sigma, rho, timeGrid)

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


Bates
*****

-----

Short Rate Models
#################

One Factor Models
*****************

- Vasicek: (r0=0.05, a=0.1, b=0.05, sigma=0.01, lambda=0.0)
- BlackKarasinski:  (YieldTermStructure, a=0.1, sigma=0.1)
- HullWhite: (YieldTermStructure, a=0.1, sigma=0.01)
- Gsr()


Vasicek
-------

.. function:: ql.Vasicek(r0=0.05, a=0.1, b=0.05, sigma=0.01, lambda=0.0)


BlackKarasinski
---------------

.. function:: ql.BlackKarasinski(termStructure, a=0.1, sigma=0.1)


HullWhite
---------

.. function:: ql.HullWhite(termStructure, a=0.1, sigma=0.01)

Gsr
---

One factor gsr model, formulation is in forward measure.

.. function:: ql.Gsr(termStruncture, volstepdates, volatilities, reversions)



Two Factor Models
*****************

G2
--


.. function:: ql.G2(termStructure, a=0.1, sigma=0.01, b=0.1, eta=0.01, rho=-0.75)

