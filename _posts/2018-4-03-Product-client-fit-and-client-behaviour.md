
<p><font size="2">This post is work in progress.<br />
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

<p>Some code can be found <a href="https://github.com/xenia-sh/product_client_fit">here.</a></font></p>


