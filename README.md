# Weighted Monte Carlo Sensitivity Calculation


The model is a non-parametric approach to value complex CDO structures that need to be priced using the market information on tranche losses at multiple points of time. Currently, the model is being used for the valuation of forward starting CDO trades (FSCDO) and loss-trigger leverage super senior tranches (LT-LSS). 

In the model, a more robust and efficient method is employed to compute the sensitivities of the risk factors that can be replicated by calibrating instruments. For FSCDO trades and LT-LSS trades it can be used to compute credit spread sensitivities and correlation sensitivities. The model is also improved to handle bucketed credit spread sensitivities and bucketed default sensitivities. The credit spread sensitivity, default sensitivity, correlation sensitivity, and interest rate sensitivity for FSCDO trades have been implemented in the model.

The model serves the purpose of computing MTM and risk for the trades priced using weighted Monte Carlo simulation (WMC) model [3].  The WMC model is a non-parametric approach to value complex CDO structures that need to be priced using the market information of the tranche losses at multiple maturities. At the present time, the model is used for the valuation of forward starting CDO trades (FSCDO) and loss-trigger leverage super senior tranches (LT-LSS). 

Three components of the model are submitted. One is a new method of computing sensitivities of the risk factors that can be expressed as a function of the calibrating instruments. Specifically they are credit spread sensitivity and correlation sensitivity for FSCDO trades and LT-LSS trades.  Compared with the old method, which is the shock-recalibration-revaluation of the risk factor, the new method is more efficient and more robust (see https://finpricing.com/lib/FiZeroBond.html). 

The second component is the computation of the bucketed credit spread sensitivity using the new method and the default sensitivity computation using the old method (“Method = DIRECT”). This improvement of the model enables us to define appropriate risk measures for FSCDO trades. 

Four risk measures for FSCDO trades are also defined and tested in this report, described as follows.

•	Credit spread sensitivity

The credit spread sensitivity for a FSCDO trade is defined as the change of MTM when a term node of the credit spread curve is shocked up by a small amount (for example, 1 or 5 bps). In other words, the credit spread sensitivity should be a bucketed sensitivity in which each node of the credit spread is considered a risk factor. 

•	Default sensitivity

A conservative approach is proposed for the default sensitivity, due to the fact the only spot default sensitivity is reported and managed in the current risk management framework which is not appropriate for forward starting trades. For a FSCDO trade with sold protection, the default sensitivity is defined as a MTM change if an obligor defaults at the forward starting date. For a FSCDO trade with bought protection, the default sensitivity is defined as a MTM change if we assume a spot default.

•	Correlation Sensitivity

The correlation sensitivity calculated in the FSCDO trade template using the new method is the MTM change with respect to 1% up of the base correlations for all the calibration spot tranches. 

•	Interest rate sensitivity

The interest rate sensitivity is the MTM change of the trade when the interest rate is parallel shifted by a small amount (for example, 50bps). Note that the hazard rate curve is not regenerated when the interest rates are shocked. We believe that this is in line with the standard market practice.

At that time very little information on modeling of the FSCDO trade was available in either industry or academic research. In the past several months, more information is available and, as we started to trade FSCDO products, the behavior of the WMC model becomes more predictable. Therefore, the WMC model and the FSCDO model are reviewed.  The latest progress of the two models was discussed and the new limitations were added.

The model reserve procedure is also re-viewed and remains unchanged. GMRMR London and GMRMR QA have agreed that, for each FSCDO trade, two most conservative scenarios are searched by 1) using various calibration instruments with the different levels of calibration tolerance, and 2) using various copulas and correlations to generate prior MC scenario. The model risk reserve is then determined by a combination of the above two. 

It is still unclear whether a particular scenario is conservative or not. Therefore, we believe it is appropriate to stick to the initial procedure of finding them. According to our understanding to the model, possible scenarios could include:

–	The calibrating tranches are set to be the standard index tranches such as 3%, 7%, 10%, etc. for CDX, a series of tranches with the same thickness as the target FSCDO tranche,  and a series of tranchlet starting from the equity tranche.
–	The calibration error tolerance is tightened to  or more stringent.
–	Many combinations of the correlation values are tested to find the most conservative prior correlation set.
–	The default correlation model can be the normal copula model, the Poisson model, or a mixture of the two.

There have been several latest developments in both modeling and market practice. First of all, non-parametric approaches have become more popular and several new approaches are proposed based not only on normal copula model, but other types of correlation models as well [4, 5, 6].  At the same time, the weakness of this kind of approach, known as the difficulty of calibrating the entire capital structure, was also confirmed by other non-parametric approaches. In fact, these developments confirm our conclusion. The model was claimed to have the capability of being calibrated to two terms. According to this information, the model is still better in the calibration to multiple terms.

The latest progress on the WMC method is summarized in a paper published on Risk Magazine. It is interesting to note that in the model the calibration of CDS term structure serves the same purpose of their proposed “synthetic option”.  Their successful application of the WMC model to a geometric cliquet option also gives us more confidence in the WMC model.  Further testing on the calibrating instruments and its relationship to the credit spread sensitivity using the new method is. In our opinion, as we are relying on the calibrating instruments to give us credit spread sensitivities and correlation sensitivities, we should be more careful in choosing the calibrating instruments.

On the FSCDO part, more information is disclosed and some research papers can be found. Two types of forward starting CDO trade are discussed which can not be priced using the current model. It seems necessary to ignore the defaults before the forward starting date, which is defined as type II FSCDO. By doing this the investor can really make the investment on the shape of the credit spread term structure and the base correlation term structure, like all other forward contracts in the fixed income world. It is interesting that they mentioned another type of forward contract, in which the collateral pool is unknown before the forward starting date. In the model, a bottom up approach is adopted where the credit spread curve of each reference obligor has to be clearly given. Therefore we think it would be inappropriate to apply the model to this kind of trade. An additional limitation is added the exclude this kind of trade from using the model. 

We assume that an asset pool is defined as a set of M obligors, , in which each obligor is described by a credit spread curve  , a hazard rate curve  , a recovery rate  , a notional amount  , and a default time  .  For each credit spread curve there is a standard term nodes, which are 1w, 1m, 3m, 6m, 1y, 2y, 3y, 4y, 5y, 10y, and 20y. 

Taking this asset pool as collateral, we define a FSCDO trade and an LT-LSS trade. The FSCDO trade is a tranche with an attachment point A, a detachment point D, and a forward starting time S, fixed time horizon , and forward starting fee payments at interval .  Assume that an LT-LSS trade has an underlying super senior tranche with an attachment point A, a detachment D, and a b/e spread s. The trade is defined by a maturity T, a time varying loss trigger denoted by K(t), a traded spread  , a leverage ratio Lev,  and the cash flows associated with the fixed fee occur at intervals  (maturity).

The spot accumulated loss function of the reference pool at time t can be written as follow:

(1)	 ,

 is the associated loss given default of the nth obligor. 

For simplicity, we assume that we have generated   MC paths and the weight of each path has been successfully calibrated. The technical details of how this is achieved can be found in Ref. [1].

The expected value for a payoff   which is a function of expected loss of the collateral pool can be written as 

(2)		 .

  is the probability weight for path  , which is different from   and computed by WMC method.    

Consider   benchmark instruments  .  Let the value of the   benchmark instrument in the   simulation path be denoted  .  Then, in order to match the benchmark instruments, as required,  we must have

(3)		  

for all  .

As proposed in Ref. [10], the sensitivity of calibrating instruments can be calculated using “the chain rule”, differentiating first with respect to the Lagrange multipliers  . More precisely,

(4)	 

A straightforward calculation shows that

(5)	  

and since our calibrating instruments are given by equation (3), then we have

(6)	  

from which we have the result that

(7)	 

This implies that the sensitivities with respect to the calibrating insturments are the linear regression corefficients of V with respect to  . Namely, if we compute

(8)	  


then we find that

(9)	 

and

(10)	 .

The WMC model is employed to price the FSCDO trades and LT-LSS trades. Different payoff function and different sets of the calibrating instruments are employed for the FSCDO trade and LT-LSS trade.  For an FSCDO trade starting from S to T, the payoff function reads:

(11)	 .

For LT-LSS, the payoff function can be written as:

(12)	 ,

with

(13)	 .

 and   are defined as the value of protection and risky annuity of the underlying super senior tranche conditional on the trigger breach, respectively.   is the default barrier breach time.

There exist many methods to minimize the objective functions shown as Eq. (8).  The L-BFGS algorithm is employed in both the submitted model and the test model. Note that the optimization method is the same as the one we used to calculate the MC path weight.  

For both FSCDO trades and LT-LSS trades, there are two sets of calibrating instruments. As discussed above, the prerequisite of the WMC is that the optimized path weights should be able to reprice the instruments that used to generate prior MC scenarios. For all the structured credit derivatives, they are defined as the unconditional default probabilities (or, equivalently, credit spread curve) of each obligor in the collateral pool. Each obligor has a term structure of the credit spread and so does the unconditional default probability. How to choose the calibrating CDS terms have been set up.

For each obligor in the collateral pool, the probability of default between the term   and  can be described by   hence the constraint is defined as

(14)			 

with

(15)			   

where   is the default time of  the   obligor in the  -th MC simulation path.  is the path weight calculated using WMC method. 

The second type of the calibrating instruments is the expected losses of a series of spot tranches. The   calibrating tranche is defined as a spot tranche with attachment point , detachment point , and maturity  . The normalized expected loss of the tranche is considered as the calibrating instrument, which denoted as

(16)	 

with

(17)	 

 is the spot accumulated tranche expected loss and  is the accumulated tranche loss for the   MC path. 

The full set of calibration instruments , shown in Eq. (3), is the union of the above two sets. Once we have successfully optimized the object function shown as Eq. (8), it is straightforward to calculate the sensitivities with respect to two sets of calibrating instruments, which are expressed as:

(17)	 

For the   obligor in the collateral pool, its credit spread sensitivity with respect term node can then be calculated by

(18)	  .

Note that both   and  , which are defined as credit spread sensitivity of default probability and spot tranche, respectively, can be calculated analytically. 

As denoted in Eq. (16), the expected loss of each calibrating tranche has to be based on base correlations associated with the attachment point and the detachment point. For the   calibrating tranche, the base correlations for this spot tranche is   and . Then within the base correlation framework the expected loss of this tranche 

(19)	 

Then the correlation sensitivity with respect to   can be expressed as

(20)	 

  is the correlation sensitivity of the spot base tranche with detachment point   and maturity  , which can be calculated readily within the current structured credit derivative modeling framework. 

The correlation reported in the model template is actually the tranche correlation by perturbing each base correlation, that is,

(21)	 ,

where  is the  calibrating tranche with attachment   and detachment point .

The tested risk measures in the submitted model include credit spread sensitivity, default sensitivity, correlation sensitivity, and interest rate sensitivity. 

•	Credit Spread Sensitivity (CS)

The credit spread sensitivity for FSCDO is defined as the change of MTM when a term node of the credit spread curve is shocked up by a small amount (for example, 1 or 5 bps). In other words, the credit spread sensitivity should be a bucketed sensitivity in which each node of the credit spread is considered a risk factor. 

Just like other forward contracts, bucketed credit spread sensitivities are the appropriate risk measure compared with a parallel shift. The forward value is very sensitive to changes in the shape of the credit spread curve term structure. As shown in the next section, the CS at the forward starting date and trade maturity are in opposite direction. 

As discussed in Refs. [1, 7] and in the last section, the arbitrage free requirement may not be satisfied at the place where not calibrated.  For FSCDO trades, this may lead to two problems. One is that the sensitivities for the terms before the forward starting dates cannot be used, unless additional calibration term added.

The second problem is that, the calibration terms may not always be the same as the credit spread terms. Therefore it is only an approximation to derive sensitivities for those calibrated terms. The control on this approximation is achieved by requiring a full term structure between the forward starting date and maturity with the maximum distance between the terms being one year. A detailed analysis has been done and is given in the next section.
 
•	Default Sensitivity (DS)

By contract, in a FSCDO trade any defaulted obligor before the forward starting date will be replaced by an AAA asset. Therefore, if we assume a spot default of an obligor and all other risk factors are held unchanged, the expected loss of the trade will be a little less than the base case. That is, if we are in the position of selling protection, we can observe negative spot default risk. 

This negative default sensitivity, although small, is dangerous from the risk management point of view. First of all, it is predicted by a static model.  In a scenario of a default event, we can expect that at least the credit spread levels of correlated obligors widen a lot, if they do not default at the same time. In the current structured credit derivate modeling framework, this feature is not captured. It is generally perceived that only a dynamical model has such ability. 

More importantly, the default risk will be incorrectly managed if we aggregate this negative value to other spot default sensitivity from spot CDO trades. It can be demonstrated in the following scenario. We can sell protection on an FSCDO equity tranche and sell a spot equity tranche. As a general risk management practice, the default sensitivities can be aggregated for these two trades. One can easily get to a situation in which we can adjust the notional amounts of the two trades such that the default risk can be canceled off. This will end up with a dangerous situation: we have no default risk, although we are selling protection on both trades!

In our opinion, the default sensitivity should be a bucketed one. An FSCDO trade is only directly subject to default risk from the forward starting date. However, in the current risk management framework, only spot default sensitivity is aggregated and managed.

Therefore, a conservative approach is proposed instead of managing forward default sensitivity. For an FSCDO trade with bought protection, the default sensitivity is calculated by assuming a spot default. For an FSCDO trade with sold protection, the default sensitivity is calculated by assuming a default at the forward starting date. Explicitly the default sensitivity of the ith obligor can be expressed as:

(22)          

It may be worthy to note that, the default sensitivity calculated by Eq.(22) may not be very robust. As a non-parametric approach, it is very important that all the calibrating instruments are self-consistent. In the computation of the default sensitivity, a large loss amount is added while the calibrating correlations are held unchanged, which may lead to a higher probability of the failure in the calibration. 

•	Correlation Sensitivity 

The correlation sensitivity calculated in the FSCDO trade template using the new method is defined as the MTM change with respect to 1% up of the base correlations for all the calibration spot tranches. 

A sample correlation results for an FSCDO trade is shown in Table 2. First, it can be seen that it is directly related to the correlations of the calibrating tranches. One base correlation shock may lead to the change of spot expected loss of two tranches. 

Furthermore, for a real FSCDO trade or LT-LSS trade, we can have up to twenty calibrating tranche. In order to manage the correlation risk with the spot tranches, a mapping from these correlation sensitivities to standard index tranche correlation sensitivities is needed.  That is the reason that we call it “raw” risk measures. In the model, the spreadsheet for this purpose has been implemented. 

•	Interest Rate Sensitivity

The interest rate sensitivity is defined as the MTM change of the trade when the interest rate is parallel shifted by a small amount (for example, 50bps).

In the model, the hazard rate curve is held unchanged when the interest rate curve is shocked. This is in line with all other structure credit derivative spot trade. In a separate analysis we did on P&L decomposition of the CDO type trades, the interest rate risk is small and it is not materially different whether the hazard rate curved is regenerated or not. We also believe that it is a convention of the market.
