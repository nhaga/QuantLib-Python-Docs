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

GeneralizedBlackScholesProcess
##############################

ExtendedOrnsteinUhlenbeckProcess
################################

ExtOUWithJumpsProcess
#####################

BlackProcess
############

Merton76Process
###############

VarianceGammaProcess
####################

GarmanKohlagenProcess
#####################

HestonProcess
#############

BatesProcess
############

HullWhiteProcess
################

HullWhiteForwardProcess
#######################

GSR Process
###########

G2Process
#########

G2ForwardProcess
################

Multiple Processes
##################

