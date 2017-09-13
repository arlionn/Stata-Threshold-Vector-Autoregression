# The Threshold Vector-autogression Program Package 

This respository contains a Stata Threshold Vector Auto-regression program package.

## .Zip Remarks
Since structure of file directories is important to the implementation of the package, programs are stored in a .zip file instead of uploading them individually.

## List of Files in TVAR.zip:
The .zip file contains:

|     | File Name                                  | Description                                                                                                      |
|-----|--------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| 1.  | prog/installation.do                       | Initialisation of the environment for the program package                                                        |
|     |                                            |                                                                                                                  |
| 2.  | prog/estimation/TVAR_2r.ado                | The main program to estimate a 2-regime TVAR                                                                     |
| 3.  | prog/help/TVAR_2r.sthlp              | The help file for TVAR_2r                                                                                        |
| 4.  | prog/estimation/TVAR_est.ado               | A helper program for estimating a TVAR model                                                                     |
| 5.  | prog/estimation/var_mle.ado                | A helper program to estimate the TVAR using the maximum likelihood                                               |
| 6.  | prog/estimation/var_ols.ado                | A helper program to estimate the TVAR using the ordinary least square                                            |
|     |                                            |                                                                                                                  |
| 7.  | prog/grid_search/grid_search.ado           | The main (wrapper) program to implement grid searches for a TVAR model                                           |
| 8.  | prog/grid_search/grid_search_op.ado        | A helper program for a TVAR model's grid searches                                                                |
| 9.  | prog/help/TVAR_2r_grid_search.sthlp | The help file for TVAR_2r_grid_search                                                                            |
|     |                                            |                                                                                                                  |
| 10. | prog/monte/bootstrap.ado                   | A program to implement bootstrapping re-sampling process                                                         |
| 11. | prog/monte/normal_simulation.ado           | A program to implement a Monte Carlo Simulation for TVAR using random variables of uniform[0,1] and Normal(0,1)  |
| 12. | prog/monte/test_normal_simulation.ado      | A wrapper program for implementing the above Monte Carlo simulation                                              |

## Example 1: Threshold Vector Autoregression with 2 Regimes ( TVAR(2) ) Estimation

Please install associated files before running examples below.

### Setup
```
. webuse set http://www.stata.com/users/dschenck/
. webuse usmacro.dta
```

Assume that observations with an odd index belongs to Regime 1. 
Other observations belong to Regime 0.
```
. gen ind = 0
. replace ind = 1 if mod(_n,2) == 1
```
Fit TVAR(2) by OLS with constant, 1 lag (the default), 0 delay (the default) and dln_m1 being the threshold variable 
from the fifth observation to the 200th observation.
```
. TVAR_2r unrate inflation in 5/200, indicator(ind) nlag(1) d(0) constant(1) ols(1)
```
Output:
```
. TVAR_2r unrate inflation in 5/200, indicator(ind) nlag(1) d(0) constant(1) ols(1)
(8 observations deleted)

Threshold Vector Autoregression with 2 Regimes ( TVAR(2) ): Estimation
Dependent Variables       : unrate inflation
Indicator Variable        : ind
Delay Parameter (d)       : 0
Number of Lags  (nlag)    : 1/1
Constant?                 : Yes
Estimation Method         : Ordinary Least Square (OLS) Estimation

Estimation Results:

---------------------------------------------------------------------------
   Regime      Parms        R^2       RMSE     Sample Size        Success?
---------------------------------------------------------------------------
        0          6     0.8927     0.7851              98             Yes
        1          6     0.9019     0.7513              98             Yes
No Regime          6     0.9641     0.4532             196             Yes
---------------------------------------------------------------------------

------------------------------------------------------------------
Regime         |  Coefficients 
---------------+--------------------------------------------------
Regime 0       |
               |
unrate:        |
_cons          |  0.5244
L1.unrate      |  0.8364
L1.inflation   |  0.1121
               |
inflation:     |
_cons          |  1.7294
L1.unrate      | -0.2636
L1.inflation   |  0.9604
               |
---------------+--------------------------------------------------
Regime 1       |
               |
unrate:        |
_cons          |  0.5231
L1.unrate      |  0.8369
L1.inflation   |  0.1119
               |
inflation:     |
_cons          |  1.7266
L1.unrate      | -0.2666
L1.inflation   |  0.9648
               |
---------------+--------------------------------------------------
No Regime      |
               |
unrate:        |
_cons          |  0.1607
L1.unrate      |  0.9369
L1.inflation   |  0.0542
               |
inflation:     |
_cons          |  0.9716
L1.unrate      | -0.1600
L1.inflation   |  0.9955
               |
------------------------------------------------------------------

. 
```
