# Automating_DCF: Automate the Discounted Cash Flow Model

The code is based on the framework written by Hugh Alessi. The code which I based the project upon can be found: https://github.com/halessi/DCF.git

If you notice any errors or have any questions/suggestions, please reach out! ***michael.jz.ma@gmail.com*** 

Hugh's code has laid a fantastic foundation and have completed most of the procedural steps in building a DCF. My work contributes upon the original framework with the goal of improving the quality of the DCF analysis.

The following are steps I am currently working on to implement:
1. Forecast the I/S, B/S, and CFS to enable a more realistic DCF model that matches industry standards.
2. Complete a dynamic WACC function which the original author left untouched.
3. Build the EBITDA multiple approach in calculating terminal value.
4. Incorporate sensitive analysis to increase the usefulness of the projections.

More ambition goals:
4. Incorporate tweaks in growth projections that are not straight-line. 
5. Calculate additional ratios so the model is not only driven by revenue and COGS growth. 
6. Build a front-end platform so users can alter what they wish to change regarding the DCF.

### Dependencies

```pip install matplotlib urllib3 seaborn```

### API Key

This project requires an API key from financialmodelingprep. You can get one for FREE 
This is an affiliate link including a coupon for premium plans for the original author halessi[here](https://intelligence.financialmodelingprep.com/pricing-plans?couponCode=halessi). 
If you choose to sign up for a non-free version due to needing more requests, you will get a ***discount*** if you use his link! It helps to compensate the original author for his time on this project. 
**disclaimer**: I am not affiliated with the original author and do not share the benefit of the discount affiliation. 

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
### Basic usage (from Hugh Alessi)

As of now, command line arguments are used to parse parameters. See main.py for default values. Here is a description of the parameters: (as of now)

```
python main.py \
        --period        
        --ticker        
        --years         
        --interval      
        --step_increase 
        --steps         
        --variable      
        --discount_rate 
        --earnings_growth_rate 
        --perpetual_growth_rate
        --apikey
```

  Argument              | Usage          
----------------------- | ------------------
period                  | how many years to directly forecast [Free Cash Flows](https://financeformulas.net/Free-Cash-Flow-to-Firm.html)
ticker                  | ticker of the company, used for pulling financials
years                   | if computing historical DCFs (i.e. years > 1), the number of years back to compute
interval                | can compute DCFs historically on either an 'annual' or 'quarter' basis. if quarter is indicated, total number of DCFS = years * 4
step_increase           | __some sensitivity analysis__: if this is specified, DCFs will be computed for default + (step_increase * interval_number), showing specifically how changing the underlying assumption impacts valuation
steps                   | number of steps to take (for step_increase)
variable                | the variable to increase each step, those available are: earnings_growth_rate, cap_ex_growth_rate, perpetual_growth_rate, discount_rate, [more to come..]
discount_rate           | specified discount_rate (W.A.C.C., it'd be nice (i think) if we dynamically calculated this)
earnings_growth_rate    | specified rate of earnings growth (EBIT)
perpetual_growth_rate   | specified rate of perpetual growth for calculating terminal value after __period__ years, EBITDA multiples coming
apikey                  | (Free) API Key to access financial data from [financialmodelingprep](https://financialmodelingprep.com/](https://intelligence.financialmodelingprep.com/pricing-plans?couponCode=halessi) -- Can also be provided as `APIKEY` envrionment variable.

### Example

If we want to examine historical DCFS for $AAPL, we can run:

```python main.py --t AAPL --i 'annual' --y 3 --eg .15 --steps 2 --s 0.1 --v eg --apikey <secret>```

or via:
```
export APIKEY=<secret>
python main.py --t AAPL --i 'annual' --y 3 --eg .15 --steps 2 --s 0.1 --v eg
```


This pulls the financials for AAPL for each year 3 years (--y) back to calculate 12 DCFs (3 years * 4 quarters), starting at a base earnings growth of 15% (--eg) and increasing for two steps (--steps) by 10% (--s), with --v specifying that earnings growth is the variable we want to increment. 

Terminal outputs some details just for us to keep an eye on:

```
Forecasting flows for 5 years out, starting at 2018-12-29. 
         DFCF   |    EBIT   |    D&A    |    CWC    |   CAP_EX   | 
2019   2.35E+10 |  2.79E+10 |  3.96E+09 |  2.17E+09 |  -3.51E+09 | 
2020   2.80E+10 |  3.70E+10 |  5.26E+09 |  1.52E+09 |  -3.82E+09 | 
2021   3.82E+10 |  5.54E+10 |  7.86E+09 |  1.06E+09 |  -4.34E+09 | 
2022   5.84E+10 |  9.19E+10 |  1.31E+10 |  7.44E+08 |  -5.12E+09 | 
2023   9.82E+10 |  1.68E+11 |  2.38E+10 |  5.21E+08 |  -6.27E+09 | 

Enterprise Value for AAPL: $1.41E+12. 
Equity Value for AAPL: $1.34E+12. 
Per share value for AAPL: $2.81E+02.
```
This provides a quick way to dive a bit deeper into what happened in the underlying calculations without necessarily needing to pull apart the code. 

### References

[1] https://github.com/halessi/DCF.git
[2] http://people.stern.nyu.edu/adamodar/pdfiles/eqnotes/dcfcf.pdf                                                      
[3] http://people.stern.nyu.edu/adamodar/pdfiles/basics.pdf                                                     
[4] https://www.oreilly.com/library/view/valuation-techniques-discounted/9781118417607/xhtml/sec30.html                     
[5] https://www.cchwebsites.com/content/calculators/BusinessValuation.html
