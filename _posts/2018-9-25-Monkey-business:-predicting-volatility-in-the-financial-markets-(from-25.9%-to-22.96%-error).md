---
<font size="2">
<p><img src="https://lh4.googleusercontent.com/73c8bYDpMPGP4Z0lebfXX_NGBdAxIIRvlk8ZsOdcke2oEAYLv92chAK8k_TyCdCepro1HBQGMKq9FxA639H3gFttHtzyaL7JYstfu-DLpo_tB_r-5WFlOQwIjt6MUGEr23gQ4HXr" style="height:160px; width:314px" /><br />
The results of the competition are now official and the winners - determined, but the game Is still on, and, moreover, some solutions have been published (therefore more possibilities to improve the first, very basic, solution from previous post). &nbsp;<br />
<br />
There are many things to do with the original script, and ideas to implement, essentially:<br />
- trying different models (other than OLS method - in published solutions I&rsquo;ve seen XGBoost, Huber regression, Weighted Linear regression, LTSM, RNN, etc.)<br />
- feature engineering (which breaks in three categories - &ldquo;by ID&rdquo;, &ldquo;by product&rdquo;, &ldquo;by date&rdquo; )<br />
- separating volatilities data from returns data and treating them separately<br />
-&nbsp;&nbsp;ensembling / stacking<br />
<br />
Different models that I&rsquo;ve tried included linear weighted regression (as in this (<a href="https://github.com/FrancoisPierre/CFM/blob/master/Starting_kit.ipynb">https://github.com/FrancoisPierre/CFM/blob/master/Starting_kit.ipynb</a> ) baseline solution), XGBoost and Huber regression (both mentioned in <a href="http://datachallenge.cfm.fr/t/proposed-solution/105">http://datachallenge.cfm.fr/t/proposed-solution/105</a> this solution).<br />
For the last two - I am surely not tuning them in a correct way, as their results for me are far from those mentioned in the article, and even quite far from weighted linear regression, for that matter.<br />
<br />
For now the returns are completely dropped out of prediction (as with them for this model the prediction is way worse then just with volatilities).<br />
Finally, the most important part is generation of new features based on &ldquo;ID&rdquo; dimension. The &ldquo;basic&rdquo; are:</p>

<ul>
	<li>
	<p>Mean volatility,</p>
	</li>
	<li>
	<p>Max volatility,</p>
	</li>
	<li>
	<p>Min volatility,</p>
	</li>
	<li>
	<p>Standard deviation (or &ldquo;volatility of volatility&rdquo;, as it&rsquo;s called in pdf above),</p>
	</li>
	<li>
	<p>Median volatility.</p>
	</li>
</ul>

<p>The non-basic ones are:</p>

<ul>
	<li>
	<p>Accumulated volatility per ID - calculated as SUM(V*P), where V, P are vectors of volatility and price for morning day ticks</p>
	</li>
	<li>
	<p>Price up max per ID - maximal volatility jump at a sequential price increase</p>
	</li>
	<li>
	<p>Price down max per ID - maximal volatility jump at a sequential price decrease</p>
	</li>
	<li>
	<p>Price up total per ID - total volatility in price up ticks</p>
	</li>
	<li>
	<p>Price down total per ID - total volatility in price down ticks</p>
	</li>
	<li>
	<p>Price up average per ID - average volatility in price up ticks</p>
	</li>
	<li>
	<p>Price down average per ID - average volatility in price down ticks</p>
	</li>
	<li>
	<p>Maximal asymmetry per ID - (Price up max - Price down max)/(Price up max + Price down max)</p>
	</li>
	<li>
	<p>Total asymmetry per ID - (Price up total - Price down total) / (Price up total + Price down total)</p>
	</li>
	<li>
	<p>Total average asymmetry per ID - (Price up average - Price down average) / (Price up average + Price down average</p>
	</li>
</ul>

<p><br />
Worth looking at correlations of these variables with each other:</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;">corr2 = input_train[cols_new].corr()<br />
sns.heatmap(corr2, xticklabels=corr2.columns, yticklabels=corr2.columns)</div>

<p><img src="https://lh4.googleusercontent.com/w631edaO57BZh2_1bxes69nc_cylgDqVXPtjqTen6brRZpTsbX3cKj4iIfdiv1qchD8C6tFQYJUUCGa5twHPTGfc7Tr00RY1bpnJxkJbEJfotsbOGpiK3WmubYJtNXt0Td0Z_3ul" style="height:286px; width:363px" /><br />
It&rsquo;s clearly visible that the block of &ldquo;up and down&rdquo; calculated variables has higher intercorrelations, and so do total and max asymmetry, consequently.<br />
Thus, the new prediction is made with all the volatilities and the newly generated variables.<br />
Weighted linear regression:</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;">regrLinWeighted = LinearRegression()<br />
regrLinWeighted.fit(input_train[cols_v1].fillna(0), output_train[&#39;TARGET&#39;], sample_weight=(1./output_train[&#39;TARGET&#39;]))<br />
output_train[&#39;Weighted_predict_v1&#39;] = regrLinWeighted.predict(input_train[cols_v1].fillna(0))</div>
<br />
<p>Here the interpolation is not used, the missing values are just filled by 0 (as when the interpolation is used first for this model, the prediction quality goes down to 23.729 instead of 23.43 which it gives when missing values are just replaced by zeros).<br />
The highest the OLS method (previously used) gives is 24.22. &nbsp;<br />
<br />
Even though returns are not used for the predictions, it&rsquo;s possible to generate some new variables with returns as well:</p>

<ul>
	<li>
	<p>Returns up per ID - number of ticks where the return goes up (of &ldquo;+1&rdquo;s)</p>
	</li>
	<li>
	<p>Returns down per ID - number of ticks where the return goes down (of &ldquo;-1&rdquo;s)</p>
	</li>
	<li>
	<p>Asymmetry of returns - (Returns up - Returns down) / (Returns up + Returns down)</p>
	</li>
</ul>



<p>Of course, I&rsquo;ve been trying other models and specifications as well and noticing some things:</p>

<ul>
	<li>
	<p>For XGBoost, the highest result I&rsquo;ve obtained is 26.96</p>
	</li>
	<li>
	<p>XGBoost seems to work a lot better with interpolation than without it (gives 27.87 with missing values just replaced by 0) &nbsp;(?)</p>
	</li>
	<li>
	<p>The question of depth of the tree/overfitting and tuning in XGBoost remains open</p>
	</li>
	<li>
	<p>For Huber regression, I&rsquo;ve got MAPE as high as 70.47 (?) for the most basic specification</p>
	</li>
	<li>
	<p>XGBoost using both returns and volatilities works better than Weighted regression using returns and volatilities (its results suffer, while for XGB they improve)</p>
	</li>
</ul>

<p><br />
Some further thoughts In terms of modelisation:</p>

<ul>
	<li>
	<p>Features importance graph is needed</p>
	</li>
	<li>
	<p>Certain features can be added still</p>
  </li>
<p><strong>Skewness and kurtosis (per ID) </strong>- idea of first place solutions here (<a href="https://github.com/cyrilledelabre/cfm-challenge/blob/master/CFM-Challenge-Example-Model.ipynb">https://github.com/cyrilledelabre/cfm-challenge/blob/master/CFM-Challenge-Example-Model.ipynb</a>)<br />
<strong>Number of NaN variables (per ID)</strong><br />
The <strong>&ldquo;return_i = volatility_i * return sign_i&rdquo;</strong> idea introduced in solution place 5 <a href="http://datachallenge.cfm.fr/t/proposed-solution/105">http://datachallenge.cfm.fr/t/proposed-solution/105</a> - <em>it has to be noted that accumulated volatility variable reflects the principle, but it&rsquo;s the total one, not &ldquo;9:30&rdquo;, &ldquo;9:35&rdquo;, &ldquo;9:40&rdquo; variables</em><br />
<strong>Average of first three volatilities</strong> (beginning of day, solution 5)<br />
<strong>Average of last six volatilities</strong> (end of afternoon, solution 5)<br />
<strong>Difference / spread between afternoon and opening </strong>- <em>this is already partially reflected in additional variables; but not quite in a way solution 5 introduces it</em><br />
<strong>Computing returns of market</strong> (averaging all stocks) and <strong>volatilities of market</strong> (per each day?) - solution 5<br />
<strong>Computing correlations between returns of every single stock with returns on the whole market , volatilities of every single stock with volatilities of market</strong> (per day?) &nbsp;- solution 5<br />
<strong>&ldquo;Product&rdquo; dimension</strong> (distinct days per product, max and min volatility per product) - from place 1 solution<br />
<strong>&ldquo;Day&rdquo; dimension</strong> (distinct products per day, &hellip;) - from place 1 solution</p>

+ Anything to be done with the outliers?
</ul>


<p>Finally, the submitted result is 22.96 on test, thus the mode has actually performed better on test then it did on train (23.4).
A pleasant, albeit unexpected, surprise.</p> </font>
