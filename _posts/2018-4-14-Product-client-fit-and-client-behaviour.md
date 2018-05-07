
<p><font size="2">
Disclaimer: The purpose of this post is to demonstrate the logic and the methodology, model choice, etc. The features and predicted products have been totally anonymized and partially scrambled by the author. Demonstrated results cannot be perceived or interpreted as real data of any financial organization.&nbsp;</font></p>

---

<p><font size="2">This case is similar to the previous one (product client fit), however, both the product and the dataset are different.<br />
In this case it&rsquo;s interesting to concentrate more on the behavior of the clients first, see what is happening if looking at time dynamic.<br />
The features and products, as before, are anonymized completely.<br />
<br />
Here I&rsquo;ll also demonstrate the logic of the SQL code I&rsquo;ve made extractions for graphs with. (All the data in examples is randomly generated.)<br />
<img src="https://lh6.googleusercontent.com/o9eLcQ6DrUgN7EviMrRGihydz6n7d30yyX3SxEf4pcEn5qq7ikMvKYhgQRQRU3DGtjW9Tss6SgAZwvut1f7RoabeB32APFI2QKe4u1J4MEjhoHTHkhgB6lNLy0UdQ7yte7uPjsrp" style="height:79px; width:369px" /><br />
This is a table of source data, from which the first idea is to<strong> group how many clients use each product to get a break down</strong>. But there&rsquo;s also time - h<strong>ow many clients start/stop using the product as time goes on</strong>? Are there any patterns?<br />
Code is straightforward:</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>SELECT &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id, product, product_name,<br />
CASE WHEN start_use &gt;= &#39;2006-01-01&#39; AND start_use &lt;= &#39;2006-01-31&#39; THEN 1 ELSE 0 END AS January06_new,<br />
CASE WHEN start_use &gt;= &#39;2006-02-01&#39; AND start_use &lt;= &#39;2006-02-28&#39; THEN 1 ELSE 0 END AS February06_new,<br />
&hellip;...<br />
CASE WHEN start_use &gt;= &#39;2006-11-01&#39; AND start_use &lt;= &#39;2006-11-30&#39; THEN 1 ELSE 0 END AS November06_new,<br />
CASE WHEN start_use &gt;= &#39;2006-12-01&#39; AND start_use &lt;= &#39;2006-12-31&#39; THEN 1 ELSE 0 END AS December06_new<br />
FROM &nbsp;products</code></div>

<p><img src="https://lh4.googleusercontent.com/CVziK0kIMvrKK6Od4LNw86kfLVchBJjJzQabGj_-Y9d4kCS7aV1YEmdsHeuGgDP6OESZBcUUtvwkM0XPh3W8iuHWJ-pTi19j6355sjmoHIlE2hJtrPPDTwaUXgI2glAYjsv-AAr0" style="height:205px; width:484px" /><br />
<img src="https://lh3.googleusercontent.com/rihR9fAH6l9m5nCJeB56th-jprWo7mM5SDCIWd_x8MTDrBwvi8TUuzXgd3agBkK9MZDfAcLxPnyspVojZ877xr8LZ-jnC-Dm7IPR3zAmB3S_TO_NNVlm8-ZCqh_is9BI7TRietHN" style="height:217px; width:469px" /><br />
First graph is the dynamic of people who joined (bought the product) each month, and<br />
We can clearly see that:</p>

<ul>
	<li>
	<p>This is data for two years and 3 months, and there definitely is a seasonal dynamic to it - clear &ldquo;downs&rdquo; are january and august;</p>
	</li>
	<li>
	<p>Product 3 basically is the same all the time with no dynamic to look at</p>
	</li>
	<li>
	<p>Something major definitely happened &nbsp;in pre-NY time, end of year 2</p>
	</li>
</ul>

<p>However, the question is - &nbsp;<strong>is this peak really so many new clients coming in</strong>?<br />
Instead, it could be:</p>

<ul>
	<li>
	<p>If there are a lot of leavers, too, then there could be a major restructuring and the contracts were re-signed (thus the peak is artificial)</p>
	</li>
	<li>
	<p>Same clients bought products for the second/third time (two credit cards, two deposits etc.)</p>
	</li>
</ul>

<p>Here for client 1 product one was bought twice (and with an overlap), and for client 2 product 2 was bought twice (without an overlap).<br />
<img src="https://lh5.googleusercontent.com/rWGAjRaeEeRPsXgn0NNXpzBrnkz76NvfGUhCzSplcUpiWLRy48t1XMhGwG4zDqQV9aj0yWBqX1-j29Ann-XfLLfDwaGJEmAjDShW17vGnORx1qK4GiMFkHk1SpO0hHEcjH8IwwZS" style="height:115px; width:355px" /><br />
	
In SQL it&rsquo;s possible to count the times client bought and dropped a particular product and if both/either were more than once, tag the client as &ldquo;multiple user&rdquo; as a separate feature in the dataset.<br />
Also, it&rsquo;s obviously worth paying attention to periods which are used for prediction - if the december is predicted, it makes more sense to predict it with december data rather then with the whole year data, for instance.

<h3><strong>The target variable</strong></h3>

<p>The setting is very similar to the &laquo;&nbsp;product client fit&nbsp;&raquo; case, however the data itself is different, also there is only one target variable predicted (one product instead of 7).<br />
<br />
The predicted product is a combination of the four products represented on the graphs above.<br />
&ldquo;Combination&rdquo; means that in the data, <strong>product 1 is, for example, Card Visa, product 2 is Card Maestro, product 3 is Mastercard Premium, and &nbsp;product 4 is Visa Platinum</strong>. If each one of them is a column taking values 0 or 1 (in this task it&rsquo;s assumed that they are mutually exhaustive), then the <strong>&ldquo;combination&rdquo; variable would be 1 if customer has at least one of these cards, and 0 if the customer doesn&rsquo;t have any</strong> of them.<br />
The features are anonymized. &nbsp;There are 4 categorical variables (categories customer is in), and ten numerical variables (anything related to their transactions, dynamics by months, overdrafts, fees they pay, etc.)</p>

<h3><strong>Data prep </strong></h3>

<p>Here there&rsquo;s no feature engineering at this point, I&rsquo;m simply dropping out the missing values, then checking there are no NaNs in any of the variables. This can be done as there were about 206&nbsp;000 lines in the dataset originally, and after dropping missing ones there are about 196 000 left (doesn&rsquo;t drastically reduce the sample).&nbsp;</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>dataset.dropna()#dropping empty values&nbsp;<br />
dataset = dataset[pd.notnull(dataset[&#39;category_feature1&#39;])] #excluding lines where there is no category_feature1<br />
for i in range(0,12):<br />
&nbsp; &nbsp;print([cols[i]])<br />
&nbsp; &nbsp;print(dataset[cols[i]].isnull().sum())<br />
&nbsp; &nbsp;print(dataset[cols[i]].count())</code></div>

<p>First, as before, correlations between variables.&nbsp;<br />
<img src="https://lh3.googleusercontent.com/a3RXgnQ8_WDvkBuqxgnexautRyTwd1Lz82ymQca6rWyrP5s2vVy5xHP72M_8mfABOAN-irATnjr3sWAt4SCvd5Ib6jsr39RoJQ8lsswTSLNxTXzBXxYTftge_4EJNyPAzcekApsY" style="height:391px; width:624px" /><br />
Here a few things are immediately evident:<br />
- Features 5,6,7 and 8,9 and 10 are very strongly correlated (knowing the dataset there&rsquo;s a simple explanation for that- this is a time dynamic of the same variable)<br />
- String correlation between numerical features 1 &amp;2 (this is in fact minimum and maximum of the same indicator, which can change for a client during his stay with the bank)<br />
- Categorical variables 1&amp; 2 and 1 &amp;4 are moderately correlated (this could be structure, meaning category 1 is the top variable, for example client types, category 2 is types by income, category 3 is types by income by region, and so forth)</p>

<h3><strong>The model(s)</strong></h3>

<p>This time, I&rsquo;m not inputting the train / test files separately, but using a more conventional method.</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>X_train, X_test, y_train, y_test = train_test_split(dataset[cols], dataset[target], test_size = 0.3, random_state=42)</code></div>

<p>As a baseline, I&rsquo;m using two models &ndash; Random Forest and XGBoost (Gradient Boosting trees, same as in the Santander case however with no pre-set parameters, as in Santander specific items such as tree depth, evaluation metric, N of rounds, etc. were indicated upon calling the classifier).</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>random_forest1 = RandomForestClassifier()<br />
random_forest1.fit(X_train, y_train.values.ravel()) #fitting<br />
y_pred= random_forest1.predict(X_test)&nbsp; #predicting<br />
print(&quot;Target score - &quot; + str(metrics.accuracy_score(y_test, y_pred)) )#model score for target vaiable</code></div>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>xgb_model1 = XGBClassifier()<br />
xgb_model1.fit(X_train, y_train.values.ravel())<br />
y_pred = xgb_model1.predict(X_test) # make predictions for test data<br />
accuracy = accuracy_score(y_test, y_pred) #accuracy score<br />
print(&quot;Target score - &quot; + str(accuracy)+&quot; for XGBoost model&quot; )#model score for target vaiable</code></div>

<p>Then, the scores.<br />
<img src="https://lh6.googleusercontent.com/gxybKkzo9PKQDUISzYQnOAyrA538QJiPGpwtcz2eBzkzVBwGpI3Y3wK4ifDLeGOODPG8YREFK67kmKQL4aSFDGbeUO--Ddo7WNwYzXVi6DK28g6mQFQBAHu5d5duWus06q1bcjBV" style="height:52px; width:360px" /><br />
Not at all bad. However, the scores alone are not enough. First of all, we have to check how strong the signal is and how many clients in our sample actually did buy the product.<br />
<img src="https://lh5.googleusercontent.com/LvKjzBOMaUZ90_NrBBILL_lP6dKRzjkgVzs3OpegL7Trb2QKKEXTaN_iuPeiSWnCyw4mAggsjRI5ApXtCvKYKTKaiq2pHXQhojQB80NeZl2-bWqegWbjnSN-cqAotnkQ-w2pZKF7" style="height:36px; width:462px" /><br />
About 23% percent of the clients, that is to say, each fifth client, has this product. Here it has to be said that <strong>for this problem only clients who do have this product now were used </strong>(not those who bought it but have an &ldquo;end date&rdquo; which is in the past, but those who use it presently).<br />
Then, variables importance.&nbsp;<br />
<img src="https://lh6.googleusercontent.com/UdhjQvid5o9rXKhZA0kusYThAqnf_pXvh9tAmKWen_be9RZGXjGOoDVw-4xxJZEK-lyfPAPFsfhyFyfpKBlaYL1hDkfYjJi2fe8EAHz_6TXH4Mizmq1MIf--M9MJGUH4Fn1-X-vm" style="height:198px; width:667px" /><br />
Here there&rsquo;s no difference in the order, but there&rsquo;s one thing capturing the attention immediately &ndash; <strong>three features (numerical features 1 to 3) do not have any importance at all</strong>. First I thought it&rsquo;s a Python mistake, but it&rsquo;s not, and we&rsquo;ll look into that later.<br />
As can be seen from the scores, XGB performs worse than Random Forest. Let&rsquo;s look at other model evaluation metrics. First, the so-called &ldquo;confusion matrix&rdquo;.<br />
The &ldquo;confusion matrix&rdquo; is read and interpreted as follows &ndash; TP (true positive, top left) are people who actually buy the product, and are predicted to buy the product correctly. FP (false positive, or type 1 error, top right) are people who did not buy the product, however were predicted to buy it. For XGB it&rsquo;s higher, which makes sense as the model score is lower (model predicts far too many people who would buy the product incorrectly). False negative, FN (bottom left, type 2 error) are people who are predicted not to buy the product but actually they did buy it. Finally, True negative is when the people did not buy the product and were predicted not to buy it.<br />
<img src="https://lh4.googleusercontent.com/TCEyOKSlSQgKWtFq4Mdf1Vb6ytdyA6NtpxBYYIuJysQqLRwJSGHL2J8UNudFB6AOG-_b4JZAsT7UMVbS2CEzQTRMWP2nw8sgHzFbe2e2JHkLZgEL_nmL7NW-2jrXE91yOH5aBb_z" style="height:83px; width:624px" /><br />
Along with that, it makes sense to construct a ROC AUC curve.<br />
The ROC curve is created by plotting the&nbsp;true positive rate&nbsp;(TPR) against the&nbsp;false positive rate&nbsp;(FPR) at various threshold settings. The true-positive rate (y axis) is also known as&nbsp;sensitivity the false-positive rate (x axis) is also known as the&nbsp;fall-out&nbsp;or&nbsp;probability of false alarm<em>. </em><br />
TPR is calculated by dividing true positive results (42346 for rf) on sum of first column of confusion matrix (everyone buying the product &ndash; 42346+6398 for rf).<br />
FPR is calculated by dividing false positive results (2582 for rf) on sum of second column of confusion matrix(everyone who did not buy the product &ndash; 2582+7977 for rf).<br />
AUC is the area under the curve.</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>#ROC AUC curve for random forest model and for XGBoost model - together<br />
y_pred_prob = random_forest1.predict_proba(X_test)[:,1]&nbsp; # Compute predicted probabilities: y_pred_prob<br />
fpr, tpr, thresholds = roc_curve(y_test, y_pred_prob) # Generate ROC curve values: fpr, tpr, thresholds RF<br />
y_pred_prob1 = xgb_model1.predict_proba(X_test)[:,1]&nbsp; # Compute predicted probabilities: y_pred_prob<br />
fpr1, tpr1, thresholds1 = roc_curve(y_test, y_pred_prob1) # Generate ROC curve values: fpr, tpr, thresholds XGB<br />
# Plot ROC curve<br />
plt.plot([0, 1], [0, 1], &#39;k--&#39;)<br />
plt.plot(fpr, tpr, color=&#39;red&#39;)<br />
plt.plot(fpr1, tpr1, color=&#39;blue&#39;)<br />
plt.xlabel(&#39;False Positive Rate&#39;)<br />
plt.ylabel(&#39;True Positive Rate&#39;)<br />
plt.title(&#39;ROC Curves (red for Random Forest, blue for XGBoost)&#39;)<br />
plt.show()</code></div>

<p><img src="https://lh5.googleusercontent.com/S6QpG_f6d4EEwwS7dO2slCxnOpvJ4PoFX6jqnKYJg081R_qaMuz0Wy7I2iZj26rtqc3UwDKmw3AwgdnBKu7u6DAgbhdaIOdWn2kbgS3GjZBjsVT-aWraMDSxomWeFV25VQGnuJIf" /><br />
One of model validation tools that I haven&rsquo;t yet shown in the blog, but an important and widely used one, is cross validation. It&rsquo;s regarded as a solution to overfitting. In cross-validation, fixed number of folds (or partitions) of the data is made, then the analysis on each fold is run, and then average the overall error estimate is produced. The question is &ndash; what is the optimal number of folds?</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>#Cross validation for random forest&nbsp;<br />
cv_scores_rd10 = cross_val_score(random_forest1,dataset[cols],dataset[target],cv=10)&nbsp;<br />
print(&quot;Average 10-Fold CV Score for random forest: {}&quot;.format(np.mean(cv_scores_rd10)))<br />
cv_scores_rd20 = cross_val_score(random_forest1,dataset[cols],dataset[target],cv=20)&nbsp;<br />
print(&quot;Average 20-Fold CV Score for random forest: {}&quot;.format(np.mean(cv_scores_rd20)))<br />
cv_scores_rd30 = cross_val_score(random_forest1,dataset[cols],dataset[target],cv=30)&nbsp;<br />
print(&quot;Average 30-Fold CV Score for random forest: {}&quot;.format(np.mean(cv_scores_rd30)))</code></div>

<p>Random forest:<br />
<img src="https://lh4.googleusercontent.com/jau4WqpGNHJ9BZaJJ6kn_zVBF70MbGVrW0qLRb4vFBRZjNWF2_nGja1zU9GvksXCbFoXh_x_Fu5-WmJtIkgB1CZXNXQIShrddaRxeXpjLiFSYrtyyYJbdqhU22MLW4mtUoKO_J1c" style="height:59px; width:394px" /><br />
XGBoost:<br />
<img src="https://lh3.googleusercontent.com/qyp6jU-NCE7FaZMyefSVDZsVOgdwmydzf3mVfCfSkAXbZAP11xo_S_3uEcGU6gMaQvA6AroE95-o00TTprSLF8fYKHvNiLaOuEtx1in2QoRAnKcCSmoZdR_wEP5KO7NbyiokecQO" style="height:64px; width:398px" /></p>



<p>One thing that can be seen from this is that by increasing N of folds the score increases (being higher for RF than for XGB all the way).</p>

<h3><strong>Features with no predictive power</strong></h3>

<p>Back to the graph with three features that have no importance. To find out the reasons, what I did is consecutively run the model and constructed the graph first for those three, then for four, then five, adding variables to see when the importance drops to 0.<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<img src="https://lh5.googleusercontent.com/7fPly3SaWMji3Jr9F3b9EeluDPMK_kzHrYP1gRfZ9qEjgro-LJVl5u6b9PziRHI9WkoiesYkw-5KVE3HBBqS6QKzBE7Vs8eqCCNMtY8E7wjvE0giPnSnG3aZ9-Ba257lCVrb65kt" style="height:495px; width:624px" /><br />
Up to 11 variables, these three (or two as first and second are max and min of the same variable and strongly correlated) have importance, however, along with category feature 4 and numeric features 5 to 6, they lose their importance. This is something to investigate.</p>

<p>Some code can be found <a href="https://github.com/xenia-sh/product_client_fit">here.</a></p>

