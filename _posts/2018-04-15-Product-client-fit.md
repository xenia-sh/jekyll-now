---
<font size = "2">
<p><em>Disclaimer: The purpose of this post is to demonstrate the logic and the methodology, model choice, etc. The data has been partially generated/scrambled by the author. Demonstrated results cannot be perceived or interpreted as real data of any financial organization. </em></p>
 
<p>How, where, when and why clients choose financial products? This is a question managers, analysts and &ldquo;conseillers d&rsquo;affaires&rdquo; ask themselves all the time, as this is what the revenue will ultimately depend on. Equally important is whether the client will leave, at which moment and why (as then incentives to make him stay can be found). &nbsp;</p>

<p>&ldquo;Products&rdquo;, as in Santander case, will be the predicted variables, and these can be anything from specific card types, to insurances, to accounts, to services for business. <a href="https://particuliers.societegenerale.fr/tous_les_produits.html">Here</a> are examples of all products by a typical French bank. </p>

<p>It&rsquo;s good to know that there are three major client categories for banks in France - particuliers, professionnels and entreprises. There are also others depending on bank policy and specification.

<h3><strong>The data </strong></h3>

<p>Our train dataset is for year N while our test dataset is for the next year (N+1).<br />
That means that when train is 2011, test is 2012, when train is 1998, test would be 1999, and so forth.<br />
<br />
Also, as will be seen by the data, the period for train is longer than for test. For example, train - 8 months, test - 2 next months, and so forth. This is a disadvantage as <strong>train and test do not have the same length and also we are not anticipating cyclical changes (predicting June with June, like in Santander). </strong><br />
<br />
We take 7 products and 11 to 13 characteristics/factors that could possibly explain them.<br />
Our goal, as in Santander challenge, is to predict whether these products will be bought or not, with maximum probability.<br />
An important difference from Santander - in that challenge prediction was on existing customers, and here everyone in &ldquo;test&rdquo; dataset is new. That is to say, if the test dataset is for February and March 1999, they came into the bank in February and March 1999. In the train dataset, however, clients who are in it were with the bank before and did not come in the train period. What we are doing is basically <strong>trying to predict the behavior of new clients based on behavior of older ones. </strong><br />
<br />
One line is one client (not a situation where one client has several lines, thus more than one set of features). The total size of the train sample is about 30000 lines/clients, the total size of the test sample - about 8000 lines/clients. Products are anonymized, and so are features. &nbsp;</p>

<h3><strong>The data - 2 </strong></h3>

<p><br />
First, we can look at how the features are distributed:</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>for i in range(0,12):<br />
&nbsp;fig = plt.figure(figsize=(10,10))<br />
ax = fig.add_subplot(111)<br />
plt.yscale(&#39;log&#39;)<br />
dataset_test[cols[i]].hist(bins=100)<br />
dataset_train[cols[i]].hist(bins=100, ax=ax, label=&#39;train&#39;, color=&#39;blue&#39;)<br />
#test and train are shown on the same graph<br />
dataset_test[cols[i]].hist(bins=100, ax=ax, label=&#39;test&#39;, color=&#39;red&#39;)<br />
legend = ax.legend()<br />
plt.title(cols[i])<br />
plt.yscale(&#39;log&#39;) #log scale of the axis<br />
plt.xscale(&#39;log&#39;)</code></div>

<p><img src="https://raw.githubusercontent.com/xenia-sh/xenia-sh.github.io/blob/master/images/graphs_features_1.png" />
<img src="https://raw.githubusercontent.com/xenia-sh/xenia-sh.github.io/blob/master/images/graphs_features_2.png" />
<img src="https://raw.githubusercontent.com/xenia-sh/xenia-sh.github.io/blob/master/images/graphs_features_3.png" />
<img src="https://raw.githubusercontent.com/xenia-sh/xenia-sh.github.io/blob/master/images/graphs_features_4.png" />
<br />
From that already we can see that most of the variables are categories - features one to 5 are. Also there&rsquo;s a very strong correlation between features 10 and 11 both in test (red) and in train datasets. Also it&rsquo;s visually clear that dataset for train is bigger in volumes as dataset test. Visualisation of the correlation matrix (higher correlations are highlighted):</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>plt.matshow(dataset_train[cols].corr()) #the correlation matrix of features<br />
print(dataset_train[cols].corr()) #correlation values</code></div>

<p><img src="https://lh6.googleusercontent.com/vFM6t3cVI2yeR8O3-vGObKGtuG06pxFEocGTltgQAHWjuuIowulSfT2-WsClz_v3dBNtnid-O1Cyi0jCRLGYb9jLvdmDD-uOJtK0DTe09-3Dz9t_qKzQjG7cswGQt_IdD6xijVE2" style="height:210px; width:202px" /><img src="https://lh4.googleusercontent.com/EOY04GovHNHOjtCvxRRG4XZAyLx-hcrXW1tnlpl3fBFji0NQVFHB4E7WKd0Vc3lao8OmHcKIYx36kGvm7_AT4oyEfVA2QDs7aIqXOMf6KYT3zysPiQL-us2T2IOr7PFgGKOMFQYs" style="height:167px; width:104px" />&nbsp; &nbsp; &nbsp;<img src="https://lh6.googleusercontent.com/2KDB2rgIcvvU8EbDzo4p1s7Ns2moIoe9AD2EGawro6igOARe7AiSlYRvGzAjRUw1TqINRRoa8TRO8wnH6oELDS5CktgvHgtwfocXg_SXQw4DReAfLWeATLjErMKEZhigNOsU8a5N" style="height:174px; width:383px" /><br />
Indeed, there is a strong correlation between features 10 and 11 (they&rsquo;re numerical values and &nbsp;nearly identical in fact), also between feature 1 and feature 4 (they are category values and so it could be that this is one category in the other or two related categories).<br />
Also there is a strong correlation between feature 12 which is a category feature and feature 9 which is numerical.<br />
There is something up with feature 3, too - test and train dataset are drastically different and that is why whether it&rsquo;s sensible to use in in training the model.<br />
Interesting.</p>

<p>For this task I&rsquo;ve decided to use three models, all giving 0 or 1 as output.<br />
The first one is Random Forest, the second one is Logistic Regression (logit) and the third one is Naive Bayes.<br />
Cycle is probably a better solution, but they&rsquo;re used consecutively on each product. Below is example for product one.</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>random_forest1 = RandomForestClassifier()<br />
random_forest1.fit(X_train, Y_train_product_1.values.ravel())<br />
Y_pred_randomforest_1 = random_forest1.predict(X_test)<br />
print(&quot;product 1 - &quot;+str(metrics.accuracy_score(Y_real_product_1, Y_pred_randomforest_1)) )#model score product 1</code></div>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>logreg = LogisticRegression()<br />
logreg.fit(X_train, Y_train_product_1.values.ravel())<br />
Y_pred_logreg_1 = logreg.predict(X_test)<br />
print(&quot;product 1 - &quot;+str(metrics.accuracy_score(Y_real_product_1, Y_pred_logreg_1)) )#model score product 1 </code></div>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>gnb = GaussianNB()<br />
gnb.fit(X_train, Y_train_product_1.values.ravel())<br />
Y_pred_GNB_1=gnb.predict(X_test)<br />
print(&quot;product 1 - &quot;+str(metrics.accuracy_score(Y_real_product_1, Y_pred_GNB_1)) )</code></div>

<p><img src="https://lh6.googleusercontent.com/XNfkUX0hvP8pExLuel6rAZJziQUbO-7sfkMerCb0C9S41slem2iVPy8QbUFLq7XiEU1F5ZUTP_nzfbLlVgAjKpyOuoKcq1JC-xmgnpLXW2XCTssDkUfcLjR6xKH9YRqLRhAin4GL" style="height:320px; width:234px" /><br />
Let&rsquo;s also look at the signal itself (= how many users actually bought each product in the test dataset). &nbsp;</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>print (&quot;product 1 - &quot;+str(sum(dataset_test[&quot;PRODUCT ONE&quot;])/len(Y_real_product_1))+ &quot; percent of customers bought it&quot;)</code></div>

<p><img src="https://lh6.googleusercontent.com/HeK8UYbhgvAWev4Q5wZuD8GhIhmBVhumLD1aQSp_p9iEnrRhSxZJ2mbqOEjjeivO6JDHqU_cbDuE7Zpq419jjdDDfPdzRCmax6Yxu3-RZoUnpVKIu5SSCGZUYu2eXfQxAogrk9Uj" style="height:108px; width:352px" /><br />
Apparently, <strong>products 1 and 4 have too small a signal to be considered</strong>. If a small percentage of clients bought the product, say, 1 in 100, if the prediction is that no clients bought the product (all 0s) is already 99% accurate&hellip;. Which is not what we want. The equivalent example is when everyone on the Titanic is dead, it&rsquo;s already 60% accuracy.<br />
On <strong>products 3 and 5</strong> it&rsquo;s the other way around - almost everyone buys them so it&rsquo;s difficult to understand which predictors are important - force of the model is not that high. But we continue with them still.<br />
Also we can <strong>remove Naive Bayes</strong> as its results are worse than two previous models. &nbsp;</p>

<h3><strong>Features importance </strong></h3>

<p>On <strong>product 6</strong> the situation is most interesting - <strong>Logistic Regression gives significantly better results than Random Forest model - 0,95 against 0,88</strong>. Worth investigating further. Maybe look at the factors it takes into account?</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><br />
<code>colors=np.array([&#39;purple&#39;, &#39;blue&#39;, &#39;r&#39;, &#39;pink&#39;, &#39;brown&#39;, &#39;y&#39;, &#39;g&#39;, &#39;lime&#39;, &#39;b&#39;, &#39;indigo&#39;, &#39;orange&#39;, &#39;grey&#39;,&#39;black&#39;])<br />
columns=np.array([&#39;feature1&#39;, &#39;feature2&#39;, &#39;feature3&#39;, &#39;feature4&#39;, &#39;feature5&#39;, &#39;feature6&#39;, &#39;feature7&#39;, &#39;feature8&#39;,<br />
&nbsp;&#39;feature9&#39;, &nbsp;&#39;feature10&#39;, &#39;feature11&#39;, &#39;feature12&#39;, &#39;feature13&#39;])<br />
features=np.array(cols)</code></div>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>importances6 = random_forest6.feature_importances_<br />
indices6 = np.argsort(importances6)<br />
plt.title(&#39;product 6 - random forest&#39;)<br />
plt.barh(range(len(indices6)), importances6[indices6], color=colors[indices6])<br />
plt.yticks(range(len(indices6)), features[indices6])<br />
plt.show()</code></div>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>indices6_logreg = np.argsort(logreg6.coef_)<br />
importances6_logreg=indices6_logreg[0]<br />
#print(len(indices6_logreg), indices6_logreg[0]) #first element because it was array n an array<br />
plt.title(&#39;product 6 - logistic regression&#39;)<br />
plt.barh(range(13), importances6[importances6_logreg], color=colors[importances6_logreg])<br />
plt.yticks(range(13), features[importances6_logreg])<br />
plt.show()</code></div>

<p><img src="https://raw.githubusercontent.com/xenia-sh/xenia-sh.github.io/blob/master/images/features_6_part1.png" />
<p><img src="https://raw.githubusercontent.com/xenia-sh/xenia-sh.github.io/blob/master/images/features_6_part2.png" />
	<br />
Here it&rsquo;s clear that the factors importance for these models is not drastically different - the weights are very close for each feature (the first graph is sorted, while the second one is not, this is why the order is different).<br />
<br />
Let&rsquo;s see for the random forest model - how important are different features for predicting each product (bearing in mind that features 10 and 11 are practically identical, and feature 3 for test dataset is very unlike train dataset).</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>fig, axes = plt.subplots(2,3,figsize=(16, 18))<br />
plt.subplots_adjust(hspace=0.4, wspace=1)</code></div>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>#features=np.array(cols)<br />
importances1 = random_forest1.feature_importances_<br />
indices = np.array(np.argsort(importances1))<br />
<br />
#one of the six graphs - product one<br />
axes[0, 0].set_title(&#39;PRODUCT ONE&#39;)<br />
axes[0, 0].barh(range(len(indices)), importances1[indices], color=colors[indices])<br />
plt.sca(axes[0,0])<br />
plt.yticks(range(len(indices)), features[indices])</code></div>

<p><img src="https://lh6.googleusercontent.com/PpdrxrE0Gwo9oDWkQf1yuiGYiAGVQBlc0QoLhHdMe74pYtbWobLZ0-OXdtZqZ5vQMI9PI5FnzWbRcuW7hjTIvHXUlM8rrzvJ4jp1r3ts1yo2gqllCXKP6-xQ9K9W6ne_DL0R4rvl" style="height:237px; width:500px" /><br />
<img src="https://lh5.googleusercontent.com/LFmZRUzvGc0zTcSS6_knORvD6WkpcWGAs-_B70wiYPzBm_Rc_o1PdYvrIMGmiT0bWT5NxwDcDolvSa6dfg42L02kwVsfDX-UHuQ3F-XeaB0u8VqMrWmJmj2y8_rRnMHaPm9imZ2D" style="height:229px; width:497px" /><br />
Products 1-4 have a similar importance of features, while products 5 and 7 (the sixth was represented above) have a different thing going on - for product five the most important feature by far is feature3, and for product seven it&rsquo;s the same but with feature4. It&rsquo;s logical to assume that these products are meant for a specific client category (persioneers, young people, teenagers, travelers etc.)<br />
It&rsquo;s also notable that the most important features for majority of the products are numeric (financial) ones, thus how the client is using the offered services is in most cases more important than in what category she is.</p>

<h3><strong>Features engineering </strong></h3>

<p>As there are many categorical variables here, it&rsquo;s maybe wirth generating additional variables which would be 0 or 1 for each variable, thus if, for example, feature1 has categories 23, 25, 26, &nbsp;then there would be new columns feature1_23, feature1_25, feature1_26 which would be 1 or 0 depending on value in feature1.</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>for i in feature4_unique:<br />
&nbsp;cn=&#39;feature4_&#39; + str(i)<br />
&nbsp;cols.append(cn)<br />
&nbsp;dataset_train[cn]=np.where(dataset_train[&#39;feature4&#39;]==i,1,0)<br />
dataset_test[cn]=np.where(dataset_test[&#39;feature4&#39;]==i,1,0)</code></div>

<p>Results (products 1 and 4 excluded as signal too feeble):<br />
<img src="https://lh6.googleusercontent.com/trgpKp9xX1lfXxgIcRDbrGgw5iubC2OD31IrV64hjeh5YtQVdaUKB-z5OAji0QdxCC-6VL0_i-VEmZFgSF0zF9wSkSbAQw3tKL-1p9uLgyNiWzWtzwgAe1bgzxB0uJzZxcudYr9e" style="height:120px; width:326px" /><br />
Product 2 more or less the same, products 3, 5 and 6 very slightly improved, product 7 slightly worse. Not the result we were hoping for. &nbsp;</p>

<h3><strong>The split &nbsp;</strong></h3>

<p>The last idea is to separate clients into groups using the variables provided. We could try doing it with each variable in turn and running the models on groups until they give a different enough result.<br />
Here we will separate them by variable feature1. It could be the volume of operations each month; it could be the type - an individual or a company; it could be whether this is their main bank or not; it could be the age, as older customers do not behave and have the same tariffs as younger ones.<br />
<br />
Here the category1 are those who have codes 68,66,67 in feature1, and category2 are those who have 64, 65, 77,91,70 or 0.</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>dataset_train_category1=dataset_train[(dataset_train[&#39;feature1&#39;]==68) | (dataset_train[&#39;feature1&#39;]==67) |(dataset_train[&#39;feature1&#39;]==66)]<br />
dataset_train_category2=dataset_train[(dataset_train[&#39;feature1&#39;]==0) | (dataset_train[&#39;feature1&#39;]==64) |(dataset_train[&#39;feature1&#39;]==65)|(dataset_train[&#39;feature1&#39;]==70)|(dataset_train[&#39;feature1&#39;]==77)|(dataset_train[&#39;feature1&#39;]==91)]<br />
dataset_test_category1=dataset_test[(dataset_test[&#39;feature1&#39;]==68) | (dataset_test[&#39;feature1&#39;]==67) | (dataset_test[&#39;feature1&#39;]==66)]<br />
dataset_test_category2=dataset_test[(dataset_test[&#39;feature1&#39;]==0) | (dataset_test[&#39;feature1&#39;]==64) |(dataset_test[&#39;feature1&#39;]==65) | (dataset_test[&#39;feature1&#39;]==70) | (dataset_test[&#39;feature1&#39;]==77) | (dataset_test[&#39;feature1&#39;]==91)]</code></div>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>X_train_category1=dataset_train_category1[cols_initial]<br />
Y_train_category1_product_2=dataset_train_category1[product_2]<br />
<br />
n3=200<br />
random_forest2_category1 = RandomForestClassifier(n_estimators=n3)<br />
random_forest2_category1.fit(X_train_category1, Y_train_category1_product_2.values.ravel())<br />
Y_pred_randomforest_2 = random_forest2_category1.predict(X_test_category1)<br />
print(&quot;product 2 - &quot; + str(metrics.accuracy_score(Y_real_category1_product_2, Y_pred_randomforest_2)) )</code></div>

<p>Results:</p>



<p><img src="https://lh3.googleusercontent.com/hq4xv3dUcJ0UFHUfQCPFYzfFX1so9OBYT4WsijeFeEiF6W74SzoFXUQ0cJ4B_kl_cl7oas-_Dot00Ixfw3qHk26QJ1_HKIPqU9wtOlUj5u2Tj3tepugvVUiDujcmAWeKYa_TEchz" style="height:227px; width:380px" /></p>



<p>What a difference! On every single product the model works better for category 2, for some (product 3 and product five, the more &ldquo;crowded&rdquo; ones) significantly better. &nbsp;<br />
Let&rsquo;s take a look at the signal by category:</p>



<p><img src="https://raw.githubusercontent.com/xenia-sh/xenia-sh.github.io/blob/master/images/features_last.png" /><br />
For products 2, 6 and 7 there is practically nobody buying them in category 2.<br />
<strong>Ideas</strong><br />
<br />
This is by no means the end of exploration, products 3 and 5 in particular can be investigated further, but a few other ideas I&rsquo;ve had (which I&rsquo;ll probably describe in this blog once it&rsquo;s done): &nbsp;</p>

<ul>
	<li>
	<p>Investigate on how frequently different products are bought with each other</p>
	</li>
</ul>

<ul>
	<li>
	<p>Limit the period of train set the same way as in Santander problem</p>
	</li>
	<li>
	<p>Look at the dynamics of product buys (also similar to Santander)</p>
	</li>
	<li>
	<p>Use a different algorithm (XGBoost) or a combination of a few</p>
	</li>
</ul>

<p>All the code for this task can be found on <a href="https://github.com/xenia-sh/product_client_fit">my Github.</a></font></p>
