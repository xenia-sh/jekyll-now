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

<p><img src="https://lh6.googleusercontent.com/2-KSyLiiAeRCMy58Oqp2RS0LygoN-NyUwrx0_FjWbJviT8oEkQbyQOJ-9f_MVXIQ4wDU63Zsd5dM9miJ3Fxmho8JxfFrXeZpYr8UAbnYYIsZPZtrq28qjzqzzTRH4hTQ8Bmd0ed5" style="height:259px; width:265px" /><img src="https://lh3.googleusercontent.com/gPjzK4kNKErP23jvLxu8YLIxcYJmQgVDTV6gmPN7XqjAolbaxAv-vKo2lca0neyJvRbzZbUpdzIKBRz_l-unEZF1S8Q2vnUqYXUt35iGNDU0klfZcV79qUAdsT07hgbVBXcpaHkK" style="height:247px; width:262px" /><img src="https://lh3.googleusercontent.com/neL0818IwssqRDZtvknPcBn-nEX3g3wzjZBRpUqfwSEKVks6HXnDfy8dITKY6Fx1gdDYGHNiiqc19ETIEYY_6k_P7i80SfUjYR3q6mty_APS0b1K9_IANQkCRg9lFv_alNK7scoN" style="height:227px; width:235px" /><img src="https://lh3.googleusercontent.com/fyjO465jCcizIimkWMFzmEOzKlJmdEYMlCYez4SuK5DMY9eJQEbLKfPsUAL5xRKHA1R4DZTHzvSs3DuWRyTSj1cKng4UW0dreKbvaUMqjhYtaAzqtzLSRFEiprmPivs_Egd3dXSQ" style="height:231px; width:232px" /><img src="https://lh4.googleusercontent.com/iCCe3yv03ITI8niS035u3QGMqEIF9flT2RjIF6TIshf06d6IBnJF8riZbssSTj5FN9sDqJ1uIiNqB5pXtF0dYjZh8gCarmK5BMvou4R67SlMRRsPdFHiAVjbEy-_LOG2Hf4j3VqY" style="height:232px; width:232px" /><img src="https://lh6.googleusercontent.com/_VGg05tvNk2HQb-VVdx_LPbGUXsMcLTrWfOK-mokyR9PwjA-XJy3ebo9qWH9jtxswIKBV3pwTeDYW2hN4-yqkzsihl3C4ZCyhTCwrEDYqjepr6HiKlSc3AOhXOANajnXW0nxtUl4" style="height:208px; width:208px" /><img src="https://lh4.googleusercontent.com/szY8rZ-gqEDot3gYJnBMokFFfCp0RCogQkMIJW3LxDEBepQy2rxbv6drQKRPTHF8TKxBldTM57gTmGMOc1Qns_oxGSUpGPcLftWhesmLEUOGQLtrAGV9HbqU_0HUMpP2_TaIwnLD" style="height:193px; width:190px" /><br />
<img src="https://lh4.googleusercontent.com/LT9tTp-ZJsyjNrGt6ZEUIn49EhB4QKM5Zl8dyXAF8yCZgc7EVlJP8Rg2zTly6l6ND-NQTgXNwWQKR5eU629vkRIW1hrA2l13dsgT6jK_rDCm2xmqjV8LnyiZNA-0it9cyl9Ag2SZ" style="height:226px; width:228px" /><img src="https://lh6.googleusercontent.com/7bPVd4D0rQqTXyggbtwOUj4rkTfWzc0g4w3mOD0DwiJT22Jgf6d9Y0ecDpL3llir8Cp42XN2-OV0bqNg93nvLUEs1VvMOM7WdvZ9NZbZXEqurmnANZIFZF7KVmZy1dLohbEbq9MS" style="height:229px; width:236px" /><img src="https://lh4.googleusercontent.com/uAzzK8hsRzNbwzjgL8-hUXnGgBE0WrY8ypWb57JSTaEWNhDGYzhy0xEn30IJnAa7LW6DmgBqOTco3r6FRagvPNhPiUMDxKSCwxxP0X01e3P9E3MqxOAoj2Iqer6UgHBNAwyMbqvG" style="height:201px; width:204px" /><img src="https://lh4.googleusercontent.com/bXOyTPjTCLZwaOWd-2qORA6Xj3TUKowb5fq7yufIw1vOwlXknarKo4FLMJKnhS7FS_z3-RzKI8E49yLMDYGDxUCXJVHD6JEjvSOZ8LrBeFEQpPDGOioIwk4qDi1N6kdn16cWCE2R" style="height:193px; width:197px" /><br />
From that already we can see that most of the variables are categories - features one to 5 are. Also there&rsquo;s a very strong correlation between features 10 and 11 both in test (red) and in train datasets. Also it&rsquo;s visually clear that dataset for train is bigger in volumes as dataset test. Visualisation of the correlation matrix (higher correlations are highlighted):</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>plt.matshow(dataset_train[cols].corr()) #the correlation matrix of features<br />
print(dataset_train[cols].corr()) #correlation values</code></div>

<p><img src="https://lh6.googleusercontent.com/vFM6t3cVI2yeR8O3-vGObKGtuG06pxFEocGTltgQAHWjuuIowulSfT2-WsClz_v3dBNtnid-O1Cyi0jCRLGYb9jLvdmDD-uOJtK0DTe09-3Dz9t_qKzQjG7cswGQt_IdD6xijVE2" style="height:210px; width:202px" /><img src="https://lh4.googleusercontent.com/EOY04GovHNHOjtCvxRRG4XZAyLx-hcrXW1tnlpl3fBFji0NQVFHB4E7WKd0Vc3lao8OmHcKIYx36kGvm7_AT4oyEfVA2QDs7aIqXOMf6KYT3zysPiQL-us2T2IOr7PFgGKOMFQYs" style="height:167px; width:104px" />&nbsp; &nbsp; &nbsp;<img src="https://lh6.googleusercontent.com/2KDB2rgIcvvU8EbDzo4p1s7Ns2moIoe9AD2EGawro6igOARe7AiSlYRvGzAjRUw1TqINRRoa8TRO8wnH6oELDS5CktgvHgtwfocXg_SXQw4DReAfLWeATLjErMKEZhigNOsU8a5N" style="height:174px; width:383px" /><br />
Indeed, there is a strong correlation between features 10 and 11 (they&rsquo;re numerical values and &nbsp;nearly identical in fact), also between feature 1 and feature 4 (they are category values and so it could be that this is one category in the other or two related categories).<br />
Also there is a strong correlation between feature 12 which is a category feature and feature 9 which is numerical.<br />
There is something up with feature 3, too - test and train dataset are drastically different and that is why whether it&rsquo;s sensible to use in in training the model.<br />
Interesting.</font></p>


