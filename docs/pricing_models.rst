##############
Pricing Models
##############


Equity
######

Heston
******

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

