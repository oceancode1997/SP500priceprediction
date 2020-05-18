**Time Series Analysis of S&P 500 Index ** 

**Abstract:** 

The S&P 500 price index indicates the performance of 500 large
businesses in the United States. It is also a big indicator of how the
United States economy is performing overall. The dataset we are working
with contains raw daily un-adjusted closing stock prices from 2 January
1986 until 28 June 2018. The objective of this study is to analyze the
performance of the S&P 500, to find an appropriate time series model for
the S&P, and to forecast based on the fitted time series model. 

**Introduction: ** 

Investing for retirement has become an incredibly important part of
American life over the past century. In order to make informed decisions
about investing in the American economy, many people turn to the S&P 500
index, which reflects the performance of 500 top businesses in the
United States, and more generally, the performance of the entire US
economy. Many investors often design a passive fund replicating an S&P
500 for long term investment.  

Using S&P 500 as a mean to estimate how much return a person can get
from investing is a good idea because the S&P 500 is a reliable measure
of the US economy and the index data is published daily and can be
obtained easily.  

In this time series analysis, we analyze the log transformed S&P 500
price and the log return of S&P 500 with various time series model and
make forecasts based on the forecast accuracy. Our analysis shows that a
SETAR model performs the best in predicting the cumulative log return of
the S&P 500.

 

**Data:** 

We download the data from the website, Kaggle. The data consists of
daily index prices from 2 January 1986 until 28 June 2018. We decide to
take the first data point from every month, since this reflects the
monthly performance of the S&P 500 and allows us to compare the data
with monthly data points from the same time period that describe the
Unemployment rate, the Consumer Price Index (CPI), the US Dollar Index,
the Crude Oil Price and the 10 year Treasury Note .The plot of the
monthly data with ACF and PACF is shown in Figure 1. 

![](media/image1.tiff){width="3.686492782152231in"
height="2.5659722222222223in"}

Figure 1: Index price from 1986 to 2018, ACF and PACF 

**Preprocessing: **

From the index, we can observe a non-constant variance, so we have
decided to compute the log transformation. This has a physical meaning,
since when we take difference in log price within 2-time stamps, we get
the cumulative log return within that period. The box-cox function gives
us a lambda of 0.08033, which indicates that log transformation is the
right approach to stabilize the variance, which will help to achieve
stationarity.  

![/var/folders/qr/x9z5m4d105z16rlbdl0cxr880000gn/T/com.microsoft.Word/Content.MSO/C7E14BC2.tmp](media/image2.png){width="5.226919291338582in"
height="3.236111111111111in"} 

Figure 2: Plot, ACF, and PACF of log transformation of S&P 500 

**Data split: **

We split the data into training and testing data. The training data is
composed of 378 points from January 1986 to June 2017. The testing data,
12 points, is from July 2017 to June 2018. Our purpose is to find out if
we can predict the cumulative log return of a year given the cumulative
data. 

**Methods:** 

1.  Time series regression 

> We first found multiple indexes in the same period that could be
> related to the S&P500 index, which are the US unemployment rate, the
> US dollar index, the CPI, and the Price of Crude Oil and the 10-year
> treasury notes. In order to achieve residuals that resembled white
> noise, we had to do a regression based on the log difference, or log
> return of the S&P 500. Then we tried to fit regression models
> on combinations of these parameters. We chose the model with the
> smallest AICc and CV, which is a linear regression based on the
> parameters of US dollar index, CPI, and 10-year treasury notes. The
> model's residuals are white noise, based on the residuals' plots and
> the p-value of Ljung-box test. The model's equation is as follows
> where α=US dollar index, β=CPI, and $\gamma$=10-year Treasury Note:

$$y_{t} = 0.46599 - 0.0009430 - 0.0031852\alpha - 0.0031852\beta - 0.0020219\gamma + .0010815t$$

![/var/folders/qr/x9z5m4d105z16rlbdl0cxr880000gn/T/com.microsoft.Word/Content.MSO/EB7E2120.tmp](media/image3.png){width="4.531492782152231in"
height="2.8055555555555554in"} 

<https://www.reddit.com/r/DotA2/comments/g2rxuj/eternalenvy_buys_bkb_midfight_to_save_his_rapier/>. Graphs
of Parameters from Jan 1986-Jun 2018 

2.  ETS: 

> Both the ets function and a manual search through all ETS models have
> (A, A, N) as the model with the lowest AICc. Checking the residuals,
> we can conclude that the residual is white noise based on the p-value
> for the Ljung-Box test and the ACF. 

The equation of ETS is the (A, A, N) with parameters:   

> ![/var/folders/qr/x9z5m4d105z16rlbdl0cxr880000gn/T/com.microsoft.Word/Content.MSO/E944C42E.tmp](media/image4.png){width="3.25in"
> height="0.2361111111111111in"} 

3.  ARIMA: The ARIMA model we choose is (1, 1, 1) with drift. We found
    > this result both by using the auto.arima() function and by
    > manually searching through all Arima models with d=1
    > for p,q=0,1,2,3,4,5. This model has the lowest AICc, and looking
    > at the residuals, we can see both through the ACF and p-value of
    > the Ljuang-Box test,(p=.6164) that the residuals resemble white
    > noise, so we can move forward with this model.  

> The equation for ARIMA model is: 

![/var/folders/qr/x9z5m4d105z16rlbdl0cxr880000gn/T/com.microsoft.Word/Content.MSO/C10F706C.tmp](media/image5.png){width="4.208333333333333in"
height="0.3057327209098863in"} 

4.  GARCH: The GARCH(Generalised Autoregressive Conditional
    > Heteroskedasticity)  model takes into account the non-constant
    > variance of time series. Because the squared residuals of our
    > ARIMA model show autocorrelation, we should add a GARCH effect to
    > our ARIMA model. We will use the log return to build
    > an ARMA(1,1) + GARCH(1,1) model. The following is the equation for
    > our model: 

> ![/var/folders/qr/x9z5m4d105z16rlbdl0cxr880000gn/T/com.microsoft.Word/Content.MSO/4B516D5A.tmp](media/image6.png){width="4.243055555555555in"
> height="0.6527777777777778in"} 
>
>  

5.  Neural network: For neural network, there is no statistical meaning
    > to the model, so we choose the model based on the lowest
    > AIC. Checking models with different numbers of nodes in the
    > initial and hidden layer lead us to adopt
    > the model nnetTs 8,6). Because this model relies on randomness,
    > with a different seed we could obtain different results both for
    > the AIC and for the actual prediction. 

>  

6.  Threshold AR (TAR, Setar): For financial time series, the recession
    > periods matter a lot. In our data, we have two noticeable
    > downtrends around 2004 and 2008.  In this case, we try to
    > implement a Threshold AR (2) model with a threshold log return of
    > --0.015 to separate recession and normal periods. The model
    > indicates that it has a white noise residual and good MSE of
    > 0.014. The following is the equation for our model:

$y_{t} = 0.064 + \left( 0.1609B - 0.0286B^{2} \right)y_{t} + \varepsilon_{t}$
for $y_{t - 1} < - 0.016$

$y_{t} = - 0.0147 + \left( - 0.0512B - 0.4351B^{2} \right)y_{t} + \varepsilon_{t}$
for $y_{t - 1} \geq - 0.016$

>  

 

**Forecast Analysis and Model Comparison: ** 

Forecast errors and AIC of the different methods are reported in Table
1: 

Table 1.

                      MSE      AIC
  ------------------- -------- ----------
  Linear regression   0.0306   -2330.59
  ETS                 0.0187   -81.27
  ARIMA               0.0199   -1254.81
  Neural network      0.0879   -2242.66
  GARCH               0.0171   -1321.54
  SETAR               0.0144   -2242.78

 

The reason ETS has significantly different AIC from Linear Regression,
ARIMA, GARCH, and SETAR is because ETS uses the log price, and the other
models require stationarity, so they use the log return. It is difficult
to compare AIC from one model to another, but we can see that SETAR
outperforms all other models, both in terms of AIC and MSE.

SETAR performs the best out of the forecasting models based on the MSE,
and we can see in the Figure 4 below that the forecasted values of SETAR
more closely align to the log of the index price for the S&P 500 during
the test data period (July 2017-June 2018) : 

![/var/folders/qr/x9z5m4d105z16rlbdl0cxr880000gn/T/com.microsoft.Word/Content.MSO/AF6166D0.tmp](media/image7.png){width="5.763888888888889in"
height="3.5694444444444446in"}  

Figure 4: Test Data and Model Predictions for Jul 2017 through Jun 2018 

 

**Conclusion:** 

We first chose the best model of each method based on AIC, these AIC
values are shown in Table 1. We checked each of the residuals for these
models and saw that they resembled white noise, and, based on the
p-values for the Ljung-Box test, we saw that the residuals were not
autocorrelated for any of the selected models. Next, we compared the
forecast error based on the MSE from each ideal model to pick the best
forecasting model.

Among these, SETAR outperformed all other models. This makes sense,
since it can divide the data into two regimes, one that reflects when
the market is doing well, and one that reflects when the market is doing
poorly, and make adjustments accordingly.  

Table 2. Cumulative Log Return of SETAR and Test data 

                  Cumulative log return    Percent Cumulative Log Ret 
  --------------- ------------------------ -----------------------------
  SETAR           .09022                   9.022 
  Actual Value    .11808                   11.808 

 

Using SETAR to make predictions, we can predict the cumulative log
return in the time period from June 2017 to June 2018, seen in Table 2
above.

For people looking to invest, this analysis shows that the economy in
the US is growing at a healthy rate, and that investing in an index that
resembles the S&P 500 should result in a good growth of returns based on
historical data and our forecasting.

 

References:  

[[https://towardsdatascience.com/beating-the-s-p500-using-machine-learning-c5d2f5a19211]{.underline}](https://towardsdatascience.com/beating-the-s-p500-using-machine-learning-c5d2f5a19211) 

[[https://www.quantstart.com/articles/ARIMA-GARCH-Trading-Strategy-on-the-SP500-Stock-Market-Index-Using-R]{.underline}](https://www.quantstart.com/articles/ARIMA-GARCH-Trading-Strategy-on-the-SP500-Stock-Market-Index-Using-R) 

[[https://www.quantstart.com/articles/Generalised-Autoregressive-Conditional-Heteroskedasticity-GARCH-p-q-Models-for-Time-Series-Analysis]{.underline}](https://www.quantstart.com/articles/Generalised-Autoregressive-Conditional-Heteroskedasticity-GARCH-p-q-Models-for-Time-Series-Analysis) 

Appendix 

Residuals Figures: 

Figure A.1 

![/var/folders/qr/x9z5m4d105z16rlbdl0cxr880000gn/T/com.microsoft.Word/Content.MSO/F5D9055E.tmp](media/image8.png){width="4.486626202974628in"
height="2.7777777777777777in"} 

Figure A.2 

![/var/folders/qr/x9z5m4d105z16rlbdl0cxr880000gn/T/com.microsoft.Word/Content.MSO/E8DBB91C.tmp](media/image9.png){width="4.553926071741032in"
height="2.8194444444444446in"} 

Figure A.3 

![/var/folders/qr/x9z5m4d105z16rlbdl0cxr880000gn/T/com.microsoft.Word/Content.MSO/B1CCA98A.tmp](media/image10.png){width="4.64365813648294in"
height="2.875in"} 

Figure A.4 GARCH Residuals 

![/var/folders/qr/x9z5m4d105z16rlbdl0cxr880000gn/T/com.microsoft.Word/Content.MSO/6952EA28.tmp](media/image11.png){width="4.666091426071741in"
height="2.888888888888889in"} 

Figure A.5 Neural Network Residuals 

![/var/folders/qr/x9z5m4d105z16rlbdl0cxr880000gn/T/com.microsoft.Word/Content.MSO/24BC5A76.tmp](media/image12.png){width="4.150129046369204in"
height="2.5694444444444446in"} 

Figure A.6 SETAR Residuals 

![/var/folders/qr/x9z5m4d105z16rlbdl0cxr880000gn/T/com.microsoft.Word/Content.MSO/54C385F4.tmp](media/image13.png){width="5.002588582677165in"
height="3.0972222222222223in"} 

Figure A.7 ACF of ARIMA Residuals \^2 showing GARCH effect

![](media/image14.tiff){width="4.236111111111111in"
height="2.5708519247594053in"}
