
<p><img src="https://lh6.googleusercontent.com/bnm6m5Lp1Kzi9Ab23Xb0-QRAjUOUOqxEwBMbX8vindEX_n0ycBGbi26dU4Kk4X5Rb6xse-IH956B6CnWxM_wYwo2fjxxiE1FXZwnoxKEg4MP195QktT-h4ujfbZ5sAmkvxjfBj81" style="height:160px; width:314px" /><br />
This post reflects work in progress, and ideas that are being tested/implemented at the moment. Meanwhile the competition has had a new best result (score below 20%).<br />

---
The essence is feature engineering, as not all possible add-ons to the dataset have been explored yet. The main focus is on day-level measures and product-level measures (as now the data is &ldquo;product-day&rdquo;, it&rsquo;s worth grouping it as many of other participants mention).</p>



<p><strong>Features engineering</strong><br />
<br />
The <strong>features</strong> that I&rsquo;m adding for <strong>day-level</strong> are:</p>

<ul>
	<li>
	<p>N of unique days for the product (how many days the product is present on the market)</p>
	</li>
	<li>
	<p>Min of market (minimum for the whole market for that day)</p>
	</li>
	<li>
	<p>Max of market ( maximum for the whole market for that day)</p>
	</li>
	<li>
	<p>Volatility of market (here a few possible ways to calculate; either mean of means or mean of ending/beginning volatilities for the day)</p>
	</li>
	<li>
	<p>N of empty volatilities in the day (total)</p>
	</li>
	<li>
	<p>N of empty returns in the day (total)</p>
	</li>
	<li>
	<p>Average spread for the day (mean of differences between the beginning and ending value of volatilities for all the stocks on any given day)</p>
	</li>
	<li>
	<p>Max spread for the day (the maximal difference between beginning and ending value of volatility on any given day)</p>
	</li>
	<li>
	<p>Min spread for the day (the minimum difference between beginning and ending value of volatility on any given day)</p>
	</li>
	<li>
	<p>Average first three volatilities for the market (for all the stocks of the day, mean of means for first three volatilities - opening)</p>
	</li>
	<li>
	<p>Average of last six volatilities for the market (for all the stocks of the day, mean of means for last six volatilities - closing)</p>
	</li>
	<li>
	<p>Other parameters related to up/down ticks, up/down vol. movements and their quantity (all average for the whole market for each day)</p>
	</li>
</ul>

<p>The <strong>features</strong> that I&rsquo;m adding for <strong>product-level</strong> are:</p>

<ul>
	<li>
	<p>N of unique products in a day (how many products are present in each day)</p>
	</li>
	<li>
	<p>Minimum volatility for the product for all time (all days in the sample)</p>
	</li>
	<li>
	<p>Maximum volatility for the product for all time (all days in the sample)</p>
	</li>
	<li>
	<p>Average volatility for the product for all time (mean of means for volatilities for all time)</p>
	</li>
	<li>
	<p>Number of total empty volatilities (how many empty values are there)</p>
	</li>
	<li>
	<p>Number of total empty returns (how many returns are there)</p>
	</li>
	<li>
	<p>Average spread for all days (mean of spreads for all days in the sample)</p>
	</li>
	<li>
	<p>Minimal spread for product (all days)</p>
	</li>
	<li>
	<p>Maximal spread for product (all days)</p>
	</li>
	<li>
	<p>Average first three volatilities for the product (all days)</p>
	</li>
	<li>
	<p>Average last &nbsp;six volatilities for the market (all days)</p>
	</li>
	<li>
	<p>Other parameters related to up/down ticks, up/down vol. Movements and their quantity (all average for the product, for all days) &nbsp;</p>
	</li>
</ul>



<p>One other measure that could be used is correlation of volatility of market and volatility of the stock for every given day (but this one has not been added yet).</p>



<p>First look at distribution per date and per product (these are both representing the simplest average measures):</p>



<p><strong>PRODUCTS &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DATES</strong><br />

<p><img src="https://lh6.googleusercontent.com/hCUMFlDe_v8-IwFMWisoQMDuZduD9huPbFRhJhXpmCynN_KJWUmgBOsNg4xLUHT0BnGeGhgAdgxZ5qV4DI3dLdjucWRcqdxmhjv04rYmNFr83V5nA4uiWd_rhjc5BnQIbRX4yvAt" style="height:170px; width:367px" /> &nbsp;<img src="https://lh5.googleusercontent.com/sGKG4zguTEp4TTbjdToQhU_Iyin_36taKbDApyO8ckU-jWb_s4z_82CKDf4uZdnvq3J6fRCIf2tK_YfBSQK-J3hVfHNFDTpHsjTBYc3Bdf3Sx-0sofyjzrFbIEJW8RKzZDaiEfjx" style="height:178px; width:390px" /> &nbsp;&nbsp;<img src="https://lh4.googleusercontent.com/mXbu_Bs5unpWmZSy03mj4YkmabZXZeB9CMto6xefF1w8eE84rN1wDjvpFvQoVHHBJflvQkgIS97ffYKkGLzFWN9oInNXynuMi1MnuToMFADG2ByuNByG45V2yf0d6cQm47pjhqjI" style="height:157px; width:384px" /><img src="https://lh4.googleusercontent.com/-Eo27dVXp7TSNFzjzWBP8pEu5aCu7-Ai0_deb3qApwTRgnWs5akepMEzyTznJAN4ECdMS1p0FF_bv2WZzEQNUAKpD4ldjR2LcdBXk_AWwDnNo7NuFffytVBAUNqE00Fe8CxPcwd0" style="height:154px; width:378px" /></p>


<p><strong>Layering </strong></p>



<p>The model currently has layers:<br />
&nbsp;</p>

<p><img src="https://lh4.googleusercontent.com/j45zptslvbfwXDcPaMDf1UxUxnGp97KdQ_azn7Vy7DJNlHLyXj4TC4NP17-aVUHxgQOhA_58eu5lEC103H6bugZWPiZ9I-Kx2zD0pU-vjCCXlLzXIIZF0WOHetb2daQzQcGL2Uo1" style="height:537px; width:624px" /></p>

<p><strong>Features importance and model results progress</strong></p>



<p>Obviously, that is way too many variables to &ldquo;throw at the model&rdquo;. But we want to see the big picture first.<br />
So, with &ldquo;generated first&rdquo; (with no returns included + this is the XGBoost model graph)<br />
<img src="https://lh4.googleusercontent.com/CI-zbfuLtEMS1exbu1E0FudstixV72Oo_h0ksXyXWsoszLJUJHPWUtE-aHPUMoDwbuJmgeLX4Mg3_zqmZ4L8m_q37JMWVHv7eLTmlBg6AKrSw62GcArfBrM9RpZtilPQNYQXBsXs" style="height:72px; width:552px" /><br />
<img src="https://lh4.googleusercontent.com/EzMrXA-071Ywo6QAJi0lGGe313JpA8Fluw4e1gPtc9xyshVzxHRW5xGJ9UBuWWttX3dDIYqV8F_bjwi_jMhjdJ7HbeIiKUy-JbnczsDKvFgfDORUnSg9xq9zi9k-_OohO1Ismaq2" style="height:777px; width:624px" /><br />
For &ldquo;generated first&rdquo; &nbsp;but with returns included, again for XGBoost (also clearly visible that overall model performance is worse):<br />
<img src="https://lh3.googleusercontent.com/DiICZlQg_pOLQf9f6_czTBsQke-LWJur6x17tGYYASmuFpDrPbbZ0YNTbpNqG9j4COPssb1HuDtPPNK_7hVxQKRp013rs7fiVs_nI_-3mivg_N3r6pttlyE1u54YSJSLRPdWj8Gb" style="height:65px; width:544px" /><br />
<img src="https://lh4.googleusercontent.com/pTrhpmdHyB6KOxUU9JhT-jALeMDeKAcEu9k-kX05HXI5DOj6zordstWs5ickegQuHplvq4QDZ1lOyVXL0t05bA1FAxJ44QGvFijjfFzl-i5NSbTd48my1r13RyR50_a-1zCnRj4r" style="height:777px; width:624px" /><br />
For &ldquo;generated second&rdquo;, again XGBoost model (it&rsquo;s improved somewhat, but still very much behind the weighted linear regression):<br />
<img src="https://lh6.googleusercontent.com/0Vkbipm1cB0VyAy_dqhKt8qndZkucRG5Xfo_tJPBSkd6BYKDRudxHrXqXXuSoLpRa_S8VA1BUK_SChRk-1aozWZkEAqLTloYrw_eliCMSvhTDCHojRM-iq44t1HufwaS7Lo5Bivk" style="height:42px; width:507px" /><br />
<img src="https://lh4.googleusercontent.com/jVFuWh4zt67zZVyfdFE4X11NKhShehiiEGtWNwjAd0_MTjbtTVvzdYInDLCGT4ZtQppT9NYv6mNvLbM-1EBJlej6eQvPwGzzIAhPjB3Z1oB5IGQ6smPwyJKsPmlBKq21yvlX3yTC" style="height:675px; width:624px" /><br />
Well maybe it&rsquo;s worth selecting just, say, 50 or 60 best parameters and seeing what happens? Even if it is just out of interest. &nbsp;<br />
As visible below, when best 40 features or so are selected, the result of XGBoost is much more robust to it as result of linear regression. The change in the error level for weighted regression here is really noticeable.<br />
<strong><img src="https://lh6.googleusercontent.com/OLHpdYbt5htBAZfQp3JaH-bt-6NvdonMxfHERiPuWjfSNMlMtHEn9ZX6X181UpQS8Ulz8CO_ymCLOjiRDMLoiuyupJSOadtXsROaLsEQsV15Bt9OlTHVAn9-_9VNMK6Ljw1k3ALb" style="height:53px; width:461px" /></strong><br />
<strong><img src="https://lh5.googleusercontent.com/pVXR6xoYrXw_-igQkF9dk5_DWWRkdVL4Z1uzw_Vt3LaNU_CgdZcA04XsK9zhGwEdSi6Vu31mQelEppnLD0VQXAeyklOxIhmI75kf2IfI-ns5xWRvCywdjAAsypyr13MAqgkcYlcl" style="height:681px; width:624px" /></strong><br />
<em>Overall, it has been said many times and is visually apparent that what is happening in the beginning of trading day and at the &ldquo;end&rdquo; (13-55) is far more important that what goes on between the two in terms of this model. Also, the market/product indicators seem to have made quite a difference.</em><br />
&nbsp;<br />
Further ideas (apart from implementing that correctly on test and submitting) are:</p>

<ul>
	<li>
	<p>Groups of products (depending on their volatility, as their spreads are very different) - creating a categorical variable, possibly</p>
	</li>
	<li>
	<p>Marking dates with volatility peaks (events on the market? As graph above demonstrates, there clearly are such dates)</p>
	</li>
	<li>
	<p><strong>Building a validation scheme</strong> in order to not have drastically different results on test and on train datasets</p>
	</li>
	<li>
	<p><strong>Finding the optimal combination of variables </strong>which to include</p>
	</li>
	<li>
	<p>Tuning in the XGBoost or <strong>ensembling a few models together &nbsp;</strong></p>
	</li>
	<li>
	<p>The market/product correlation idea</p>
	</li>
</ul>


