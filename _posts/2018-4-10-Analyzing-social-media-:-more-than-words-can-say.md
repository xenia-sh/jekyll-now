
<p><font size="2">This post is a work in progress. First of all, this is an interesting story (and an even more interesting dataset). <font>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<img src="https://lh6.googleusercontent.com/c5rXEmpnUUzvwhHicFHhVteAjGcDu-qhCpyapZ8oQOybb5kaD4PRF5xI_6_MbhAszAPTw99DE15C3qMDGGH7MJW1mzmyTjkBOU-qcSDzjXev3ln0fuvgDWatX3w8FJfXTDRiAdFa" style="height:132px; width:624px" /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br />

---
  
<font size="2">I was going through the materials to showcase in this blog and came across a presentation me and my ESCP classmates did on Microsoft. It has stock prices graphs, revenue indicators, product portfolio and many other pieces of info that we scraped across multiple public sources.<br />
First I thought about expanding on that and looking at behavior of Microsoft stocks more closely, but then, almost immediately, I had an idea - what about the people? Are people working for Microsoft any different from employees of other companies? And if they are, in what way? And this ultimately led to me searching for data of people&rsquo;s social media accounts and this study (especially since it&rsquo;s analyzing text rather than numerical values as in previous posts).<br />
<br />
Not long after I came across <a href="https://aminer.org/data-sna#Linkedin">this</a> dataset which is data on nearly 160 000 social media accounts. It&rsquo;s in MySQL Dump format, and for converting it to CSV using Python <a href="https://github.com/jamesmishra/mysqldump-to-csv">this&nbsp;tutorial</a> is quite helpful.<br />
Upon looking at what we have I&rsquo;ve found out that the tables available are &ldquo;multiple blogger&rdquo;, &ldquo;multiple facebook&rdquo;, &ldquo;multiple flickr&rdquo;, &ldquo;multiple google&rdquo;, &ldquo;multiple lastfm&rdquo;, &ldquo;multiple livejournal&rdquo; , &ldquo;multiple myspace&rdquo;, &ldquo;multiple picasaweb&rdquo;, &ldquo;multiple twitter&rdquo; and &ldquo;multiple youtube&rdquo;. So, basically, every social network out there!</font></p>

<p><font size="2">Out of these tables I chose Google, Youtube, Facebook, &nbsp;Twitter and Blogger. The great thing about this dataset is that it&rsquo;s versatile and contains truly a lot of data - but <strong>for this study the more superficial data points such as music, books, hobbies, films, emails and links</strong>, which are tailored to the person specifically, <strong>are omitted</strong>.<br />
<br />
The fields interesting for the analysis are:

<ul>
	<li>
	<p>Age</p>
	</li>
	<li>
	<p>Gender</p>
	</li>
	<li>
	<p>Location (here - country, hometown, any other location indicators)</p>
	</li>
	<li>
	<p>N of connections</p>
	</li>
	<li>
	<p>Occupation (job)</p>
	</li>
	<li>
	<p>Industry</p>
	</li>
	<li>
	<p>Company</p>
	</li>
	<li>
	<p>School / university</p>
	</li>
	<li>
	<p>[possibly] last online for proxy of an activity online</p>
	</li>
	<li>
	<p>The social media the data is taken from</p>
	</li>
</ul>

<p>So we promptly reduce the dataset:<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<img src="https://lh3.googleusercontent.com/tyESzJEJpImGKLDwfuLoEl_cFvokqlRSZaKWnWSsN0eT6PYg_9Nbd7CSumyJpgrqJJUSJ5-BrQsX4-Snh9PmCuRe6kIzqoX0BnDhLTaDv1MHc6LUmXn7TV_M8QG19LGENaZ397Fp" style="height:303px; width:624px" /><br />
What is interesting - my first assumption was that there records are completely unrelated across social networks.<br />
However, upon making one table and counting unique gids, I found out that <strong>out of 120 000 lines only 35 000 are unique ones</strong>. Which means they&rsquo; re active across a few social media which is quite common. &nbsp;<br />
<strong>So, in fact, better gather all info about one user in one line. </strong><br />
Writing a script for a &ldquo;long&rdquo; dataset and displaying results by groups we get:</p>

<ul>
	<li>
	<p>5 location variables (all 5 media)</p>
	</li>
	<li>
	<p>3 occupation/job variables</p>
	</li>
	<li>
	<p>4 companies variables and 1 industry one (all except blogger)</p>
	</li>
	<li>
	<p>3 number of connections variables</p>
	</li>
	<li>
	<p>3 schools/university variables (google, facebook and youtube)</p>
	</li>
	<li>
	<p>2 gender and 2 age variables (blogger and facebook)</p>
	</li>
</ul>

<p>There is also last activity online.</p>

<p><strong>The data</strong><br />
	
First thoughts on what to do with data:</p>

<ul>
	<li>
	<p>Join the variables in one category together, delete replicating ones</p>
	</li>
	<li>
	<p>Introduce one separator between words in one field</p>
	</li>
	<li>
	<p>Delete signs like &ldquo;?&rdquo;, &ldquo;!&rdquo;, &ldquo;@&rdquo;</p>
	</li>
	<li>
	<p>Average/max number of connections over all</p>
	</li>
	<li>
	<p>Unclear on how to account for last time online (&ldquo;3 hours ago&rdquo; or a specific day do not say anything)</p>
	</li>
	<li>
	<p>Most common locations (possibly a map), schools,companies, professions</p>
	</li>
	<li>
	<p>Industry is only indicated for 300 people so can probably be removed</p>
	</li>
</ul></font>
<p><font size="2">Let&rsquo;s take a look at the data:

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>str(DATASET_SOCIAL_MEDIA_2)</code></div>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>Classes &lsquo;tbl_df&rsquo;, &lsquo;tbl&rsquo; and &#39;data.frame&#39;:&nbsp;&nbsp; &nbsp;35265 obs. of &nbsp;27 variables:<br />
$ gid &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: chr &quot;001.deepak&quot; &quot;02chan.com&quot; &quot;03devildog&quot; &quot;0404wyt&quot; ...<br />
$ gender_facebook &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: chr NA NA &quot;Male&quot; NA ...<br />
$ location_facebook &nbsp;&nbsp;&nbsp;&nbsp;: chr NA NA &quot;San Diego California&quot; NA ...<br />
$ schools_facebook &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: chr NA NA &quot;San Diego State University \\&#39;99 &nbsp;Palomar \\&#39;73 San&quot; NA ...<br />
$ companies_facebook &nbsp;&nbsp;&nbsp;: chr NA NA &quot;Caliburnus Enterprises&quot; NA ...<br />
$ age_group_facebook &nbsp;&nbsp;&nbsp;: chr NA NA NA NA ...<br />
$ n_connections_facebook: int &nbsp;NA NA 1208 NA NA 509 NA NA 689 NA ...<br />
$ occupation_google &nbsp;&nbsp;&nbsp;&nbsp;: chr &quot;student&quot; &quot;advertiser&quot; &quot;business &amp;amp &nbsp;&nbsp;internet services &amp;amp consultin&quot; &quot;student&quot; ...<br />
$ companies_google &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: chr NA NA &quot;mcrd museum historical society &nbsp;&nbsp;decor &amp;amp styl&quot; NA ...<br />
$ schools_google &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: chr &quot;ryan international school &nbsp;delhi&quot; NA &quot;san diego state university (mba - 1999) &nbsp;&nbsp;uc san d&quot; NA ...<br />
$ organization_google &nbsp;&nbsp;: chr NA NA &quot;caliburnus enterprises&quot; NA ...<br />
$ location_google &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: chr &quot;pune india&quot; &quot;shanghai&quot; &quot;san diego &nbsp;california&quot; &quot;&amp;#38271 &amp;#27801&quot; ...<br />
$ age_youtube &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: int 23 27 NA 31 NA 24 NA NA 21 31 ...<br />
$ location_youtube &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: chr &quot;India&quot; &quot;China&quot; NA &quot;United States&quot; ...<br />
$ last_active_youtube &nbsp;&nbsp;: chr &quot;Jul 22 2011&quot; &quot;Jul 25 &nbsp;2011&quot; NA &quot;8 months ago&quot; ...<br />
$ occupation_youtube &nbsp;&nbsp;&nbsp;: chr NA NA NA NA ...<br />
$ companies_youtube &nbsp;&nbsp;&nbsp;&nbsp;: chr NA NA NA NA ...<br />
$ schools_youtube &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: chr NA NA NA NA ...<br />
$ connections_youtube &nbsp;&nbsp;: int 0 0 NA 0 8 2 0 NA 17 190 ...<br />
$ location_twitter &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: chr NA &quot;shanghai&quot; &quot;San Diego &nbsp;CA&quot; &quot;china&quot; ...<br />
$ last_active_twitter &nbsp;&nbsp;: chr NA &quot;Thu Jul 09 07:44:07 +0000 2009&quot; &quot;Sat Mar 13 00:56:05 +0000 2010&quot; &quot;Sat Sep 24 08:24:08 +0000 2011&quot; ...<br />
$ n_connections_twitter : int &nbsp;NA 31 37 119 234 NA 163 27 NA 76 ...<br />
$ gender_blogger &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: chr NA NA NA NA ...<br />
$ location_blogger &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: chr NA NA NA NA ...<br />
$ industry_blogger &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;: chr NA NA NA NA ...<br />
$ occupation_blogger &nbsp;&nbsp;&nbsp;: chr NA NA NA NA ...<br />
$ connections_total &nbsp;&nbsp;&nbsp;&nbsp;: int NA NA NA NA NA NA NA NA NA NA ...</code></div>

<p>Connections together (in every social media):</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>DATASET_SOCIAL_MEDIA_2$connections_total&lt;-DATASET_SOCIAL_MEDIA_2$n_connections_facebook+DATASET_SOCIAL_MEDIA_2$n_connections_twitter<br/>
	+DATASET_SOCIAL_MEDIA_2$connections_youtube<br />
&gt; #creating total number of connections<br />
&gt; summary(DATASET_SOCIAL_MEDIA_2$connections_total)<br />
Min. 1st Qu. &nbsp;Median Mean 3rd Qu. &nbsp;&nbsp;&nbsp;Max. NA&#39;s<br />
5 373 &nbsp;&nbsp;&nbsp;&nbsp;744 2804 1631 6148860 &nbsp;&nbsp;26840</code></div>

<p>Most of people have about 400 to 600 connections on Facebook.</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>hist(DATASET_SOCIAL_MEDIA_2$n_connections_facebook, breaks=100)</code></div>

<p><img src="https://lh5.googleusercontent.com/vf6AF5bHkH27IwMAR9E6PCBVitlMZAZ8WO9K180twZdqDCTq7p2e7rxu0eoEB194o9ctUWrQghI0eEStMVp1KaTy6Zau3ZaZVo86Sk84S02JvkOagWdoaU_VqHrtnbHsyiCaGnH9" style="height:320px; width:624px" /><br />
A curious thing is immediately clear - there are <strong>a lot more men than women</strong> for those likes who have the gender. &nbsp;80% or more of what we are seeing filled in are men.</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>&gt; length(which(DATASET_SOCIAL_MEDIA_2$gender_blogger==&quot;Female&quot;))<br />
[1] 463<br />
&gt; length(which(DATASET_SOCIAL_MEDIA_2$gender_blogger==&quot;Male&quot;))<br />
[1] 1582<br />
&gt; length(which(DATASET_SOCIAL_MEDIA_2$gender_facebook==&quot;Female&quot;))<br />
[1] 2542<br />
&gt; length(which(DATASET_SOCIAL_MEDIA_2$gender_facebook==&quot;Male&quot;))<br />
[1] 16139</code></div>

<p>Then we unite everything in one column, for example:</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>schools_cols &lt;- c(&#39;schools_facebook&#39;, &#39;schools_google&#39;, &#39;schools_youtube&#39;)<br />
&gt; DATASET_SOCIAL_MEDIA_2$schools_all &lt;- apply(DATASET_SOCIAL_MEDIA_2[, schools_cols], 1, paste, collapse = &quot; &quot;)</code></div>

<p>Replacing the NA values:</p>

<div style="background:#eee;border:1px solid #ccc;padding:5px 10px;"><code>DATASET_SOCIAL_MEDIA_2$schools_all &lt;- gsub(&#39;NA&#39;, &#39;&#39;, DATASET_SOCIAL_MEDIA_2$schools_all)</code></div>

<p><br />
The full code for this task (ongoing) can be found at <a href="https://github.com/xenia-sh/bag_of_words_socialmedia">my Github</a></font></p>.


