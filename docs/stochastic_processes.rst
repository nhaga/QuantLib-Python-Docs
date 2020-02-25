####################
Stochastic Processes
####################


GeometricBrownianMotionProcess
##############################


.. function:: ql.GeometricBrownianMotionProcess(initialValue, mu, sigma)

.. code-block:: python

  initialValue = 100
  mu = 0.01
  sigma = 0.2
  process = ql.GeometricBrownianMotionProcess(initialValue, mu, sigma)


BlackScholesProcess
###################

.. function:: ql.BlackScholesProcess(initialValue, riskFreeTS, volTS)

.. code-block:: python

  initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
  sigma = 0.2
  today = ql.Date().todaysDate()
  riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
  volTS = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), sigma, ql.Actual365Fixed()))
  process = ql.BlackScholesProcess(initialValue, riskFreeTS, volTS)


BlackScholesMertonProcess
#########################

.. function:: ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volTS)

.. code-block:: python

  initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
  sigma = 0.2
  today = ql.Date().todaysDate()
  riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
  dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
  volTS = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), sigma, ql.Actual365Fixed()))
  process = ql.BlackScholesMertonProcess(initialValue, dividendTS, riskFreeTS, volTS)


GeneralizedBlackScholesProcess
##############################

.. function:: ql.GeneralizedBlackScholesProcess(initialValue, dividendTS, riskFreeTS, volTS)

.. code-block:: python

  initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
  sigma = 0.2
  today = ql.Date().todaysDate()
  riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
  dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
  volTS = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), sigma, ql.Actual365Fixed()))
  process = ql.GeneralizedBlackScholesProcess(initialValue, dividendTS, riskFreeTS, volTS)



ExtendedOrnsteinUhlenbeckProcess
################################

.. function:: ql.ExtendedOrnsteinUhlenbeckProcess(speed, sigma, x0)

.. code-block:: python

  x0 = 0.0
  speed = 1.0
  volatility = 0.1
  process = ql.ExtendedOrnsteinUhlenbeckProcess(speed, volatility, x0, lambda x: x0)


ExtOUWithJumpsProcess
#####################

.. code-block:: python

  x0 = 0.0
  x1 = 0.0
  beta = 4.0
  eta = 4.0
  jumpIntensity = 1.0
  speed = 1.0
  volatility = 0.1
  ouProcess = ql.ExtendedOrnsteinUhlenbeckProcess(speed, volatility, x0, lambda x: x0)
  process = ql.ExtOUWithJumpsProcess(ouProcess, x1, beta, jumpIntensity, eta)

BlackProcess
############

.. function:: ql.BlackProcess(initialValue, riskFreeTS, volTS)

.. code-block:: python

  initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
  sigma = 0.2
  today = ql.Date().todaysDate()
  riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
  volTS = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), sigma, ql.Actual365Fixed()))
  process = ql.BlackProcess(initialValue, riskFreeTS, volTS)


Merton76Process
###############

.. code-block:: python

  initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
  sigma = 0.2
  today = ql.Date().todaysDate()
  dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))

  riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
  volTS = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), sigma, ql.Actual365Fixed()))
  process = ql.BlackProcess(initialValue, riskFreeTS, volTS)

  jumpIntensity = ql.QuoteHandle(ql.SimpleQuote(1.0))
  jumpVolatility = ql.QuoteHandle(ql.SimpleQuote(sigma * np.sqrt(0.25 / jumpIntensity.value())))
  meanLogJump = ql.QuoteHandle(ql.SimpleQuote(-jumpVolatility.value()*jumpVolatility.value()))

  process = ql.Merton76Process(initialValue, dividendTS, riskFreeTS, volTS, jumpIntensity, meanLogJump, jumpVolatility)

VarianceGammaProcess
####################

.. code-block:: python

  initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
  riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
  dividendTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))

  sigma = 0.2
  nu = 1
  theta = 1
  process = ql.VarianceGammaProcess(initialValue, dividendTS, riskFreeTS, sigma, nu, theta)

GarmanKohlagenProcess
#####################

.. function:: ql.GarmanKohlagenProcess(initialValue, foreignRiskFreeTS, domesticRiskFreeTS, volTS)

.. code-block:: python

  initialValue = ql.QuoteHandle(ql.SimpleQuote(100))
  domesticRiskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.03, ql.Actual365Fixed()))
  foreignRiskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.01, ql.Actual365Fixed()))
  volTS = ql.BlackVolTermStructureHandle(ql.BlackConstantVol(today, ql.NullCalendar(), sigma, ql.Actual365Fixed()))
  process = ql.GarmanKohlagenProcess(initialValue, foreignRiskFreeTS, domesticRiskFreeTS, volTS)


HestonProcess
#############

BatesProcess
############

HullWhiteProcess
################

.. function:: ql.HullWhiteProcess(riskFreeTS, a, sigma)

.. code-block:: python

  riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
  a = 0.001
  sigma = 0.1
  process = ql.HullWhiteProcess(riskFreeTS, a, sigma)


HullWhiteForwardProcess
#######################

.. function:: ql.HullWhiteForwardProcess(riskFreeTS, a, sigma)

.. code-block:: python

  riskFreeTS = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual365Fixed()))
  a = 0.001
  sigma = 0.1
  process = ql.HullWhiteForwardProcess(riskFreeTS, a, sigma)


GSR Process
###########

.. code-block:: python

  today = ql.Date().todaysDate()
  dates = [ql.TARGET().advance(today, ql.Period(i, ql.Days)) for i in range(1,10)]
  times = [i for i in range(1,10)]
  sigmas = [0.01 for i in range(0, len(dates)+1)]
  reversion = 0.01
  reversions = [reversion]
  process = ql.GsrProcess(times, sigmas, reversions)


G2Process
#########

G2ForwardProcess
################

Multiple Processes
##################

