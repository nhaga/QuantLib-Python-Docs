Black & Bachielier Engines
##########################

The price engine module provides with a set of functions from the two cornerstones models in mathematical finance: the **Black** and **Bachelier** option pricing formulas, also exposing their implied volatility inverses, and related probabilities such as *in-the-money (ITM) probabilities*.

These functions are commonly used for pricing, calibration, and risk analysis in derivatives models based on lognormal (Black) or normal (Bachelier) assumptions.

Black Related functions
***********************

.. function:: ql.blackFormula(optionType: ql.Option.Type, strike: float, forward: float, stdDev: float, discount: float = 1.0, displacement: float = 0.0)

    Computes the **Black (1976)** option price for a given forward, strike, and volatility.

    :param optionType: Type of the option (`Option.Call` or `Option.Put`).
    :type optionType: ql.Option.Type

    :param strike: Strike price of the option.
    :type strike: float

    :param forward: Forward price of the underlying.
    :type forward: float
    
    :param stdDev: Standard deviation of log returns (σ√T).
    :type stdDev: float

    :param discount: Discount factor for the payoff. Defaults to `1.0`.
    :type discount: float, optional

    :param displacement: Displacement applied to avoid negative forwards/strikes. Defaults to `0.0`.
    :type displacement: float, optional

    :returns: The Black option price.
    :rtype: float


Black Implied Volatility (Standard and LiRS methods)
----------------------------------------------------

.. function:: ql.blackFormulaImpliedStdDev(optionType: ql.Option.Type, strike: float, forward: float, blackPrice: float, discount: float = 1.0, displacement: float = 0.0, guess: float = None, accuracy: float = 1.0e-6, maxIterations: int = 100)

    Computes the **implied standard deviation** (σ√T) from a given Black option price.

    :param optionType: Option type (Call or Put).
    :type optionType: ql.Option.Type

    :param strike: Strike price.
    :type strike: float

    :param forward: Forward price of the underlying.
    :type forward: float

    :param blackPrice: Observed Black option price.
    :type blackPrice: float

    :param discount: Discount factor. Defaults to `1.0`.
    :type discount: float, optional

    :param displacement: Displacement applied to forward/strike. Defaults to `0.0`.
    :type displacement: float, optional

    :param guess: Initial guess for the implied standard deviation.
    :type guess: float, optional

    :param accuracy: Root-finding tolerance. Defaults to `1.0e-6`.
    :type accuracy: float, optional

    :param maxIterations: Maximum number of iterations. Defaults to `100`.
    :type maxIterations: int, optional

    :returns: Implied standard deviation.
    :rtype: float

.. function:: ql.blackFormulaImpliedStdDevLiRS(optionType: ql.Option.Type, strike: float, forward: float, blackPrice: float, discount: float = 1.0, displacement: float = 0.0, guess: float = None, omega: float = 1.0, accuracy: float = 1.0e-6, maxIterations: int = 100)

    Computes the implied standard deviation using the **LiRS (Li–Rydén–Ståhl)** root-finding method, an accelerated variant for faster convergence.

    :param omega: Relaxation factor for LiRS iterations (1.0 = standard Newton).
    :type omega: float, optional

Other parameters are as in :func:`blackFormulaImpliedStdDev`.


.. function:: ql.blackFormulaImpliedStdDevLiRS(payoff: ql.PlainVanillaPayoff, forward: float, blackPrice: float, discount: float = 1.0, displacement: float = 0.0, guess: float = None, omega: float = 1.0, accuracy: float = 1.0e-6, maxIterations: int = 100)
   :no-index-entry:

    Overload of the LiRS implied volatility solver that accepts a payoff object.

    :param payoff: Plain-vanilla payoff describing strike and option type.
    :type payoff: ql.PlainVanillaPayoff


Black ITM Probabilities
-----------------------

.. function:: ql.blackFormulaCashItmProbability(optionType: ql.Option.Type, strike: float, forward: float, stdDev: float, displacement: float = 0.0)

    Computes the **probability (under risk-neutral measure)** that the option ends *in the money*, expressed in cash terms.

    :returns: The discounted probability that the option is in the money.
    :rtype: float

.. function:: ql.blackFormulaCashItmProbability(payoff: ql.PlainVanillaPayoff, forward: float, stdDev: float, displacement: float = 0.0)
   :no-index-entry:

    Same as above, but accepts a payoff object.

.. function:: ql.blackFormulaAssetItmProbability(optionType: ql.Option.Type, strike: float, forward: float, stdDev: float, displacement: float = 0.0)

    Computes the **asset-based ITM probability**, used for delta and forward measure computations.

.. function:: ql.blackFormulaAssetItmProbability(payoff: ql.PlainVanillaPayoff, forward: float, stdDev: float, displacement: float = 0.0)
   :no-index-entry:

    Overload accepting a payoff object.

BlackCalculator
---------------

.. class:: ql.BlackCalculator(payoff: ql.StrikedTypePayoff, forward: float, stdDev: float, discount: float = 1.0)

    The **BlackCalculator** computes the price and Greeks of European-style options under the **Black (lognormal)** model.
    It provides analytical results for option value, delta, gamma, vega, rho, and other sensitivities based on a given payoff, forward price, and volatility.

    :param payoff: The option payoff (e.g., call or put) defining the strike and option type.
    :type payoff: ql.StrikedTypePayoff
    :param forward: The forward price of the underlying asset.
    :type forward: float
    :param stdDev: The standard deviation of log returns (σ√T).
    :type stdDev: float
    :param discount: The discount factor applied to the payoff. Defaults to `1.0`.
    :type discount: float, optional

    .. method:: value() -> float
    Returns the **option price** under the Black model.

    .. method:: deltaForward() -> float
    Returns the **forward delta**, i.e., derivative of the option value with respect to the forward price.

    .. method:: delta(spot: float) -> float
    Returns the **spot delta**, i.e., derivative of the option price with respect to the current spot price.

    .. method:: elasticityForward() -> float
    Returns the **forward elasticity**, the percentage change in price for a 1% change in the forward price.

    .. method:: elasticity(spot: float) -> float
    Returns the **spot elasticity**, the percentage change in price for a 1% change in the spot price.

    .. method:: gammaForward() -> float
    Returns the **forward gamma**, the second derivative of the option value with respect to the forward price.

    .. method:: gamma(spot: float) -> float
    Returns the **spot gamma**, the second derivative of the option value with respect to the spot price.

    .. method:: theta(spot: float, maturity: float) -> float
    Computes the **theta**, the rate of change of the option value with respect to time to maturity.

    .. method:: thetaPerDay(spot: float, maturity: float) -> float
    Returns the **theta per day**, i.e., daily time decay of the option value.

    .. method:: vega(maturity: float) -> float
    Returns the **vega**, the sensitivity of the option price to volatility.

    .. method:: rho(maturity: float) -> float
    Returns the **rho**, the sensitivity of the option price to changes in the interest rate.

    .. method:: dividendRho(maturity: float) -> float
    Returns the **dividend rho**, sensitivity to changes in the dividend yield.

    .. method:: itmCashProbability() -> float
    Returns the **cash-based probability** that the option ends in the money.

    .. method:: itmAssetProbability() -> float
    Returns the **asset-based probability** that the option ends in the money.

    .. method:: strikeSensitivity() -> float
    Returns the derivative of the option value with respect to the **strike price**.

    .. method:: strikeGamma() -> float
    Returns the **second derivative** of the option value with respect to strike.

    .. method:: alpha() -> float
    Returns the **α (alpha)** parameter of the Black model formula.

    .. method:: beta() -> float
    Returns the **β (beta)** parameter of the Black model formula.

**Example usage:**

.. code-block:: python

    payoff = ql.PlainVanillaPayoff(ql.Option.Call, 100)
    calc = ql.BlackCalculator(payoff, forward=105, stdDev=0.2, discount=0.99)
    price = calc.value()
    delta = calc.delta(spot=102)


Bachelier Related functions
***************************

.. function:: ql.bachelierBlackFormula(optionType: ql.Option.Type, strike: float, forward: float, stdDev: float, discount: float = 1.0)

    Computes the **Bachelier (Normal)** option price, assuming normal (additive) dynamics for the underlying.

    :returns: The Bachelier option price.
    :rtype: float


Bachelier Implied Volatilities
------------------------------

.. function:: ql.bachelierBlackFormulaImpliedVol(optionType: ql.Option.Type, strike: float, forward: float, tte: float, bachelierPrice: float, discount: float = 1.0)

    Computes the implied **normal volatility** from a given Bachelier option price.


.. function:: ql.bachelierBlackFormulaImpliedVolChoi(optionType: ql.Option.Type, strike: float, forward: float, tte: float, bachelierPrice: float, discount: float = 1.0)

    Alternative implied volatility computation using **Choi’s closed-form approximation** for faster evaluation.

Bachelier ITM Probabilities
---------------------------

.. function:: ql.bachelierBlackFormulaAssetItmProbability(optionType: ql.Option.Type, strike: float, forward: float, stdDev: float)

    Computes the probability (under normal dynamics) that the option finishes in the money, expressed in asset terms.

.. function:: ql.bachelierBlackFormulaAssetItmProbability(payoff: ql.PlainVanillaPayoff, forward: float, stdDev: float)
   :no-index-entry:

    Overload accepting a payoff object.

BachelierCalculator
-------------------

.. class:: ql.BachelierCalculator(payoff: ql.StrikedTypePayoff, forward: float, stdDev: float, discount: float = 1.0)

    The **BachelierCalculator** computes the price and Greeks of European-style options under the **Bachelier (normal)** model.
    It is suitable for assets or rates that can take negative values (e.g., interest rates).
    All sensitivities are derived assuming **additive** (not lognormal) dynamics.

    :param payoff: The option payoff defining the strike and option type.
    :type payoff: ql.StrikedTypePayoff
    :param forward: The forward price of the underlying asset.
    :type forward: float
    :param stdDev: The standard deviation of the underlying (σ√T).
    :type stdDev: float
    :param discount: The discount factor for present value. Defaults to `1.0`.
    :type discount: float, optional

    .. method:: value() -> float
    Returns the **option price** under the Bachelier (normal) model.

    .. method:: deltaForward() -> float
    Returns the **forward delta** under the normal model.

    .. method:: delta(spot: float) -> float
    Returns the **spot delta**, i.e., sensitivity to the current spot price.

    .. method:: elasticityForward() -> float
    Returns the **forward elasticity** (percentage change with respect to forward).

    .. method:: elasticity(spot: float) -> float
    Returns the **spot elasticity** (percentage change with respect to spot).

    .. method:: gammaForward() -> float
    Returns the **forward gamma**, second derivative with respect to forward.

    .. method:: gamma(spot: float) -> float
    Returns the **spot gamma**, second derivative with respect to spot.

    .. method:: theta(spot: float, maturity: float) -> float
    Returns the **theta**, time decay of the option value.

    .. method:: thetaPerDay(spot: float, maturity: float) -> float
    Returns **theta per day**, the daily time decay.

    .. method:: vega(maturity: float) -> float
    Returns the **vega**, sensitivity to volatility under normal dynamics.

    .. method:: rho(maturity: float) -> float
    Returns the **rho**, sensitivity to the interest rate.

    .. method:: dividendRho(maturity: float) -> float
    Returns the **dividend rho**, sensitivity to dividend yield.

    .. method:: itmCashProbability() -> float
    Returns the **cash probability** of finishing in the money.

    .. method:: itmAssetProbability() -> float
    Returns the **asset probability** of finishing in the money.

    .. method:: strikeSensitivity() -> float
    Returns the derivative of option value with respect to **strike**.

    .. method:: strikeGamma() -> float
    Returns the **second derivative** of value with respect to strike.

    .. method:: alpha() -> float
    Returns the **α (alpha)** parameter of the Bachelier formula.

    .. method:: beta() -> float
    Returns the **β (beta)** parameter of the Bachelier formula.

**Example usage:**

.. code-block:: python

    payoff = ql.PlainVanillaPayoff(ql.Option.Put, 100)
    calc = ql.BachelierCalculator(payoff, forward=98, stdDev=0.15, discount=0.995)
    price = calc.value()
    vega = calc.vega(1.0)
