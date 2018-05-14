
<p><font size="2">
<p>Curiosity killed the cat. Also, it made me register for the <a href="https://challengedata.ens.fr/en/challenge/34/volatility_prediction_in_financial_markets.html#general-information-modal">challenge</a> of predicting the volatility of selected stocks, organized this year on Challenge Data by CFM. The presentation of the challenge is available <a href="https://www.college-de-france.fr/site/stephane-mallat/Prediction-de-volatilite-de-marches-financiers-par-CFM.htm">here</a>.<br />
This post is my first steps in analysing this purely financial dataset in the data science setting.</p>
</font></p>
<p><img src="https://lh6.googleusercontent.com/SAamav86wIefuJ_Z75cCGGQlBzE5t9S4wO1zOo0nXmshp_rBT1gOysQtK6oHJRCGpZY1M34yceP24sHIQWP_L4aMvd1RU8U8S1E_3gBxzaegNBzqm_MSNEr7oSLo1zYDRh2WRJSi" style="height:160px; width:314px" /></p>


---
<p><font size="2">Financial datasets are very particular both in terms of models used and in particularities of the data. As Claudia Perlich, Senior Data Scientist at Two Sigma, pointed out in this <a href="https://www.youtube.com/watch?v=kBR0EtGOkzc">video</a>, the first thing that is extremely important is <strong>time</strong>, as these are time series data; and the second thing to think about is <strong>noise</strong>, that is to say signals within the data are not very clear.<br />
The data itself is:<br />
- ID of each line,<br />
- the date,<br />
- the product id,<br />
- volatilities for each 5-minute interval from 9-30 in the morning to 13-55 in the afternoon,<br />
- returns for each 5--minute interval from 9-30 in the morning to 13-55 in the afternoon.<br />
<br />
The target variable is volatility in two last hours of the day.<br />
<br />
Here&rsquo;s how the predictors look (54 periods in total for both volatilities and returns):</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>cols = [&quot;ID&quot;,&quot;date&quot;,&quot;product_id&quot;,&quot;volatility 09:30:00&quot;,&quot;volatility 09:35:00&quot;,&quot;volatility 09:40:00&quot;,<br />
&hellip;.<br />
&ldquo;volatility 13:40:00&quot;,&quot;volatility 13:45:00&quot;,&quot;volatility 13:50:00&quot;,&quot;volatility 13:55:00&quot;,<br />
&quot;return 09:30:00&quot;,&quot;return 09:35:00&quot;,&quot;return 09:40:00&quot;,&quot;return 09:45:00&quot;,<br />
&hellip;.<br />
&quot;return 13:40:00&quot;,&quot;return 13:45:00&quot;,&quot;return 13:50:00&quot;,&quot;return 13:55:00&quot;]</code></div>



<p>The test and train datasets are similar in size.</p>



<p><img src="https://lh6.googleusercontent.com/rywcKLm4RyCPFAT84lxGKkE6gJyR1zTbsfB98KZf4Wo4LqKtLjM0_o1_RbI4yau_dPd65dra7x_ub5bKnr50aHFStjwd_oSk4Tcsr465dFOhBMbaww9qoWIo3wONb4gC-RjpG3K9" style="height:44px; width:602px" /><br />
<img src="https://lh5.googleusercontent.com/-wzDx8-pIbSSz-8IVtQUw2CkcJY_AY-5aG3m6SjPT_uHLYZZgAcS5dlYt7wFMCLPG1skW5K59CE5-ybFxTt1OQaR7fsdJjwPVXG0_oZ2HMUSZdCULa3z9FG-xxAfIIJ3HCkU9rVi" style="height:40px; width:415px" /></p>



<p>In total, there are 318 unique products and 2117-2119 unique days (about 5,8 years of data if 365 days in a year).<br />
<br />
<img src="https://lh5.googleusercontent.com/agtarYej6qjw90C8qTrH-4dQGe6DtmA43WMhO0rmddpyDzDHARsOZ9OJp9B2Al64RbeudfS6DFC4ch8mAjs9XCN8zhTLa6agHjW98Fd3wr45y_CoTfZr3qpqKDVUtoafslo1isw_" style="height:71px; width:205px" /></p>



<p>What is important at the first glance:</p>

<ol>
	<li><strong>Dates are anonymized</strong> (not in actual date format but given as numbers) and scrambled, so the data is not ordered&nbsp; &agrave; <strong>no seasonality</strong>, not possible to &ldquo;catch&rdquo; any trends &nbsp;</li>
	<li><strong>Returns are not given as absolute numbers. </strong>Instead, they are 0 if price of stock remained the same, -1 if it went down and +1 if it went up. This is for each of 54 periods within a day</li>
	<li><strong>The score is mean absolute percentage error</strong> (MAPE) which is very different than score of, say, a random forest or gradient boosting models, because the expected output is non-binary and to check whether the prediction is accurate one needs to know the squared difference of prediction and the actual target, divided by n of periods (&ldquo;the closeness&rdquo; to the real volatility).&nbsp;</li>
</ol>

<h3><strong>Missing values </strong></h3>

<p>This is to see whether there are many missing values and if yes, how many and in which particular columns. Here&rsquo;s the number of missing values modelled (the graph &ldquo;repeats itself because on the left are volatilities and on the right are returns, and where first are missing, second are too):<br />
<img src="https://lh5.googleusercontent.com/QxuZ-BTVpSH9HOmjcgkM9cWkf0c6B5InmkmBwB2U1Hd0HmyQ0VrTrwy-5j9BqknNQxcaRuzahVqmD5vcP-r6dHxxJS9m26wF-h-79piYGJluGNnNe1Djlh-qRuOqa8LUCq4H1C-s" style="height:412px; width:837px" /><br />
<img src="https://lh6.googleusercontent.com/kGd8Qptm3x3wSF2rzw8ZcLWADQSgaQZ3xngudnqdayOUPsmOqdYM50K1UEl39ojhoJrDJT644NfSv83qn_e5PjZzbxXV9shClN3ahN-uutGZPTPaVifveFFSK88-9X2yYcGAr4u9" style="height:493px; width:624px" /><br />
<img src="https://lh5.googleusercontent.com/mv6Vd3F38yqJLtvkwuUVXTAXPwnXyZBLQ4exbcCOPiGhKkPibtwfl9O-hTzyIV0bVsQbimr2nf1ur26oilUjlRA1MtH4Ba3pIqftCa8NVuboAhXu-rvTkMIczT6G_GwsxqZNqd81" style="height:449px; width:595px" /><br />
Here it&rsquo;s clearly visible that the maximum missing values in 9-30 and 9-35 chunks (opening of the stock exchange), and also in 12-40, 12-50 and 12-55 (middle of the day). In all the other chunks, the number of missing values is relatively stable.<br />
This is either just a particularity of the dataset, or probably these were taken out intentionally.<br />
The max % of missing values is about 4,7%-5% which is generally good as it&rsquo;s not a lot.</p>

<h3><strong>First submission</strong></h3>

<p>&nbsp;For my first submission, I did the simplest thing - not yet modelising anything, I computed the mean of all the volatilities in each line.<br />
&nbsp;</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>#taking all the volatilities for each line and calculating their mean<br />
col = input_test.loc[: , &quot;volatility 09:30:00&quot;:&quot;volatility 13:55:00&quot;]</code></div>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>input_test[&#39;volatility_mean&#39;] = col.mean(axis=1)<br />
#creating a dataframe for output of a required length<br />
output_test=pd.concat([input_test[&#39;ID&#39;],input_test[&#39;volatility_mean&#39;]], axis=1)</code><br />
&nbsp;</div>

<p>Submitting it as predicted volatility, here&rsquo;s what I&rsquo;ve got:<br />
<br />
<img src="https://lh3.googleusercontent.com/gKDU-C_lD0kqq2pS0cReOAhBdS7bOM1flu5I3mqixrtWHHrBpcLvdxnz8Blc6e_ObkK_5y3VQigdOwZx8Bowo4EStekb8A9N4sa3OAOBk74jLBGXkhlyy75ThWS_EYb99w0Xu2rj" style="height:96px; width:176px" /></p>



<p> The best top-dashboard score is 20.96%.<br />
<br />
Then again, I don&rsquo;t have a model yet. J There&rsquo;s definitely room for score improvement.<br />
<br /></p>
<h3><strong>OLS method (simple linear regression)</strong></h3>

<p>Let&rsquo;s add the model then. The simplest one is linear regression, OLS (ordinary least squares) method.<br />
The first problem with it is that it does not allow empty values (they have to be replaced or removed, as in the example with the &lsquo;missing&rsquo; parameter).<br />
The second problem with it is, obviously, correlation variables (as better to check whether columns are strongly correlated or not).</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>#simple OLS model with a non-binary output<br />
model = sm.OLS(output_train[&#39;TARGET&#39;],input_train[cols], missing=&#39;drop&#39;)<br />
results=model.fit()<br />
output_train[&#39;TARGET1&#39;]&nbsp; = results.predict(input_train[cols]) #predicting on TRAIN</code></div>

<p>Doing the prediction, replacing empty values (about 1/6 of the dataset which is significant) with mean ones from the previous submission, and here&rsquo;s the result:</p>



<p><img src="https://lh3.googleusercontent.com/gkIeID_SbTnI_cxTtTLFJTGatD8amydq_LBvTCb1JZ1Z9WKxXX3B1xJdDmicXbLZRA9XSuEVMbCpKn_5Z-qbGO4GsnliPTsYKncH_5PMKN_-k4gQl4pnDlYG-EQGEqml12RA9BC8" style="height:31px; width:624px" /></p>



<p>Much better already.</p>
<p>Some code can be found <a href="https://github.com/xenia-sh/cfm_volatility_prediction">here</a>.</font></p>
