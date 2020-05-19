# S&P 500 time series analysis
Using  different time series model to find the best model that can predict the log return of SP 500. <br />
We use SP 500 data from 1986 to 2018. We split data from 1986-2017 as training data, 2017-2018 as our test data. We also see if the best model works from 2018-2019. <br />
The best model has forecast MSE of 0.0012 and accurately predicts the log return of S&P within 2 yearperiod within 2%. The model is shown below. <br />
![](SP500%20model.PNG) <br />
The full report is in file [Time Series Analysis on S&P 500.docx](https://github.com/oceancode1997/SP500priceprediction/blob/master/Time%20series%20analysis%20on%20S&P%20500.docx?raw=true) <br />
The raw SP 500 data can be downloaded [spx.csv](https://github.com/oceancode1997/SP500priceprediction/blob/master/spx.csv) <br />
THE financial data I collected for linear regression model can be downloaded in [predictors.csv](https://github.com/oceancode1997/SP500priceprediction/blob/master/predictors.csv). <br />
My code is in file Rmd:  [SP 500.Rmd](https://github.com/oceancode1997/SP500priceprediction/blob/master/SP%20500.Rmd) or the html file: [SP_500.html](https://github.com/oceancode1997/SP500priceprediction/blob/master/SP_500.html?raw=true) <br />

