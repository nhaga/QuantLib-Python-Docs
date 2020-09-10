SABR
####

SabrSmileSection
****************

.. function:: ql.SabrSmileSection(date, fwd, [alpha, beta, nu, rho], dayCounter, Real)

.. function:: ql.SabrSmileSection(time, fwd, [alpha, beta, nu, rho], dayCounter, Real)

.. code-block:: python

  alpha = 1.63
  beta = 0.6
  nu = 3.3
  rho = 0.00002

  ql.SabrSmileSection(17/365, 120, [alpha, beta, nu, rho])


sabrVolatility
**************

.. function::  ql.sabrVolatility(strike, forward, expiryTime, alpha, beta, nu, rho)

.. code-block:: python

  alpha = 1.63
  beta = 0.6
  nu = 3.3
  rho = 0.00002
  ql.sabrVolatility(106, 120, 17/365, alpha, beta, nu, rho)


shiftedSabrVolatility
*********************

.. function:: ql.shiftedSabrVolatility(strike, forward, expiryTime, alpha, beta, nu, rho, shift)

.. code-block:: python

  alpha = 1.63
  beta = 0.6
  nu = 3.3
  rho = 0.00002
  shift = 50

  ql.shiftedSabrVolatility(106, 120, 17/365, alpha, beta, nu, rho, shift)



sabrFlochKennedyVolatility
**************************

.. function:: ql.sabrFlochKennedyVolatility(strike, forward, expiryTime, alpha, beta, nu, rho)
  
.. code-block:: python
  
  alpha = 0.01
  beta = 0.01
  nu = 0.01
  rho = 0.01

  ql.sabrFlochKennedyVolatility(0.01,0.01, 5, alpha, beta, nu, rho)