---
<font size = "2"><p>This post is about a well-known case in DS community in general - <a href="https://www.kaggle.com/c/santander-product-recommendation">Santander Product Recommendation Challenge</a>.<br />
  <br />
Though I used rather than wrote the code myself (<a href="https://www.kaggle.com/apryor6/detailed-cleaning-visualization-python">kernel 1</a> &nbsp;and <a href="https://www.kaggle.com/sudalairajkumar/when-less-is-more">kernel 2</a> ) only adding a few things, this one is notable because it gave me both tools and inspiration for continuing to explore how and why bank clients choose certain products.</p>

<h3><strong>Task setting</strong></h3>
<p>I won&rsquo;t spend too much time explaining the setting and data as Kaggle and seasoned competitors do it so much better than me. In short - Santander asks to predict which additional products will customers buy in a given month (3 to 4% of them indeed buy something new in addition to what they already have, and thus the maximum prediction accuracy is 4%).<br />
<br />
The dataset is huge (it covers 1.5 years of customers behavior data starting in 2015-01-28 and ending in 2016-05-28) and has about 13 million lines, and the limitation needs to be used in order not to crash Python&rsquo;s memory, or indeed filtering by a certain column value when reading the file.<br />
<br />
<a href="https://www.kaggle.com/c/santander-product-recommendation/forums/t/25579/when-less-is-more">This</a> post changed the course of the competition - it was &nbsp;made public that <strong>prediction should be made with only data for June 2015</strong> as it would make the results (additional products bought in June 2016) significantly better.<br />
<br />
<img src="https://lh5.googleusercontent.com/R0jCJm_AsJkT9rhBOTnpbuJQ8Z4tdyJAN8rRMgKP33e-GvcEmCV4pwjTLZv7fs5jE1i-_0HUqZtHEj05YnL4gX6ggLspsKYFZgGJBoo7i_xHbELDR0_5NdCdqVJsevne34ngU_er" style="height:276px; width:470px" /><br />
This graph (taken from <a href="http://blog.kaggle.com/2017/02/22/santander-product-recommendation-competition-3rd-place-winners-interview-ryuji-sakata/">here</a>) clearly shows peaks in June 2015 and also December 2015.<br />
<strong>Data cleaning</strong><br />
<br />
There are <strong>24 variables</strong> used to predict products: fetcha_dato (the month), customer id, employee index, country of residence, gender, age, date when joined the bank, new customer index, seniority, primary index, last date as primary customer, type, relation type, residence index, foreigner index, spouse of employee index, channel, deceased, type of address, province code, province name, activity index, gross income of household, segmentation.<br />
<br />
Just a note - what is interesting there is virtually no financial information about the clients (except for their income and products they have), &nbsp;thus, nothing about how often they use bank services - the frequency, the volumes of operations, the quantity of operations. The information is mostly personal characteristics.<br />
<br />
<strong>Products are:</strong> Saving account; Guarantees; Current accounts; Derivada account; Payroll accounts; Junior account; mas/Particular/Particular plus account; Short term deposits; Medium term deposits; Long-term deposits; E-account; Funds; Mortgage; Pensions; Loans; Taxes; Credit Card; Securities; Home account; Payroll; Pensions; Direct Debit.<br />
<a href="https://www.kaggle.com/apryor6/detailed-cleaning-visualization-python">Here</a> is one of the most upvoted kernels on cleaning the data, briefly:</p>

<ul>
	<li>
	<p><strong>Age </strong>- as seen on graph below, it makes more sense taking people ages 18 to 30 and their mean for empty values, then people aged 31-100 and their mean for empty values;</p>
	</li>
	<li>
	<p><strong>New customer inde</strong>x - he looks at how many months customers for whom the variable is empty have, finds that they have 6 months and replaces them all by 1</p>
	</li>
	<li>
	<p><strong>Seniority</strong> - if lower than 0 then it is set to 0, for new customers found previously - min seniority</p>
	</li>
	<li>
	<p><strong>Date joined</strong> - median date is given for those for whom it&rsquo;s zero</p>
	</li>
	<li>
	<p><strong>Status</strong> (primary/not in beginning of month) - 1 is more common so 1 for missing values</p>
	</li>
	<li>
	<p><strong>Client activity</strong> - replaced by median &nbsp;</p>
	</li>
	<li>
	<p><strong>City</strong> - not replaced but labeled as &ldquo;UNKNOWN&rdquo;</p>
	</li>
	<li>
	<p><strong>Renta (household income)</strong> - a lot of missing values; empty ones replaced by median by province (as incomes vary considerably across provinces in the sample)</p>
	</li>
	<li>
	<p><strong>Address type</strong> - dropped &nbsp;</p>
	</li>
	<li>
	<p><strong>Province code</strong> - dropped</p>
	</li>
	<li>
	<p><strong>Product values (dependent variables)</strong> - he looks at what was most common in previous months and replaces empty values by it</p>
	</li>
</ul>

<p>An interesting graph on age - we see that there it peaks at 24-25 (young customers) and then again at 45.<br />
<img src="https://lh5.googleusercontent.com/edmcAr1_y4e5T6bhYP1EC55OQTWGuT6AL9IZSfVsQZ6z483p9teIk249NjSPR_tMmMZeij6Od9NsQBrDtMf-STM7Cq0b8Wc3DMxCDPBGtCkYVgqPpLpkr-AnI2PsUAfgUc2b88dT" style="height:228px; width:411px" /><br />
&nbsp;<br />
Now all the variables don&rsquo;t have missing values:</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>print(df.isnull().any())</code></div>

<p><img src="https://lh3.googleusercontent.com/Wykh8gfC97XmeVQV-FrFkl9i2kKiv3Vpr7fiMGRvfe94r8vEi5LFfSaNSuM_eih1LyDpdJPo8vDdUmJAC8qPzMp3RgWjRmkmezMFHXI2WE6hQna4uhhGGXchFoINtDpbNptvIvZg" style="height:322px; width:197px" /><img src="https://lh4.googleusercontent.com/XA46ua14IP3rqOruzMv7-3CDM2YP-rOa07XTs6bFOCfLvsV3AqJhfi5zT_zM7NWYVoDNLSXvD7QqObFyjqOucmO3I8EBi0_bXFA04IFeI_LYAtH3cUJolPeX12fO3_HzLQB8h7D4" style="height:296px; width:204px" /><br />
  
<h3><strong>The model</strong></h3><br />

Important - this kernel (cleaning data) does not convert text variables to numeric values, which will be a problem when/if XGBoost is used (as it works only with int, float or bool values). To solve this, numeric values are mapped onto non-numeric variables using a dictionary.<br />
<br />
The only amendments that I&rsquo;ve made are <strong>adding &lsquo;UNKNOWN&rsquo;</strong> together with &lsquo;NA&rsquo; and &lsquo;&rsquo; values as it&rsquo;s used in the first kernel a lot, and also <strong>removing age, seniority and renta /income of a household</strong> (as in the solution they are modified in a different way than in first kernel).<br />

Also here the file is read with DictReader, line by line, in a way I haven&rsquo;t used &nbsp;before. The principle is:<br />
Read row by row &rarr; for each column take its value, match with dictionary and substitute<br />

This is where the famous XGBoost, one of the most used and powerful algorithms in machine learning, comes in. The first model/predictions took about 20 min to make, but the score is not that high:<br />

<img src="https://lh3.googleusercontent.com/Do84hHZe6LH9LpENA6_q9d2STpwy5ol9vrEcpzzJ2RPbIE_iLaxspvCwgM_-2c0IQokeJoe1hAztv1c6T0GWof2tTaoTx7Sl9SXiWvcRMOTMJPNgdU2IbkqIuOG_g9HWAxgB65hg" style="height:43px; width:624px" /><br />

For the second submission I added back renta, seniority and age (although directly, not through specifying functions for each one as it&rsquo;s been done in kernel 2):</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>x_vars = []<br />
&nbsp; &nbsp;for col in cat_cols:<br />
&nbsp; &nbsp; &nbsp; x_vars.append( getIndex(row, col) ) #GetIndex is a function &nbsp;matching values with dictionary<br />
&nbsp; &nbsp; &nbsp;&nbsp;x_vars.append(int(row[&#39;age&#39;])) &nbsp;&nbsp;#add age as it&#39;s already a number with no missing values<br />
&nbsp; &nbsp; &nbsp;&nbsp;x_vars.append(int(row[&#39;antiguedad&#39;])) &nbsp;#add antiguedad /seniority<br />
&nbsp; &nbsp; &nbsp;&nbsp;x_vars.append(float(row[&#39;renta&#39;]) ) &nbsp;#add renta</code></div>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;">&nbsp;</div>
<p>Building model/predicting once again:</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>print(train_X.shape, train_y.shape)<br />
<em>(1071232, 39) (1071232,)</em><br />
print(datetime.datetime.now()-start_time)<br />
<em>0:01:00.623444</em><br />
print(test_X.shape)<br />
print(datetime.datetime.now()-start_time)<br />
<em>(929615, 39)<br />
0:01:32.127147</em><br />
Building model..<br />
Predicting..<br />
0:47:28.671259<br />
Getting the top products..<br />
0:47:42.199612</code></div>

<p>Submit the prediction.<br />
  
<img src="https://lh5.googleusercontent.com/upzYMuuNBre9TdGBQ7MoQJC88gfanjK9UiS8WZsp2m5BxLVpPmclTJyn04lKlDu7280iugAzl72BzpBaLqFLmbQpdZ0M0BSfX0NhxQRJDoEjGhtKYRNJ95GjSjt6aQBkfgzsEIKk" style="height:44px; width:624px" /><br />
  
The prediction quality has improved, but ever so slightly. 

All the code for this task is at https://github.com/xenia-sh/kaggle_miscellaneous.</font></p>
