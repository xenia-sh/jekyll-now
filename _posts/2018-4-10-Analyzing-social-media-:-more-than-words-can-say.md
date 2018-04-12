
<p><font size="2">First of all, this is an interesting story (and an even more interesting dataset)<font>.
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<img src="https://lh6.googleusercontent.com/c5rXEmpnUUzvwhHicFHhVteAjGcDu-qhCpyapZ8oQOybb5kaD4PRF5xI_6_MbhAszAPTw99DE15C3qMDGGH7MJW1mzmyTjkBOU-qcSDzjXev3ln0fuvgDWatX3w8FJfXTDRiAdFa" style="height:132px; width:624px" /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br />

---
  
<font size="2">I was going through the materials to showcase in this blog and came across a presentation me and my ESCP classmates did on Microsoft. It has stock prices graphs, revenue indicators, product portfolio and many other pieces of info that we scraped across multiple public sources.<br />
First I thought about expanding on that and looking at behavior of Microsoft stocks more closely, but then, almost immediately, I had an idea - what about the people? Are people working for Microsoft any different from employees of other companies? And if they are, in what way? And this ultimately led to me searching for data of people&rsquo;s social media accounts and this study (especially since it&rsquo;s analyzing text rather than numerical values as in previous posts).<br />
<br />
Not long after I came across <a href="https://aminer.org/data-sna#Linkedin">this</a> dataset which is data on nearly 160 000 social media accounts. It&rsquo;s in MySQL Dump format, and for converting it to CSV using Python <a href="https://github.com/jamesmishra/mysqldump-to-csv">this&nbsp;tutorial</a> is quite helpful.<br />
Upon looking at what we have I&rsquo;ve found out that the tables available are &ldquo;multiple blogger&rdquo;, &ldquo;multiple facebook&rdquo;, &ldquo;multiple flickr&rdquo;, &ldquo;multiple google&rdquo;, &ldquo;multiple lastfm&rdquo;, &ldquo;multiple livejournal&rdquo; , &ldquo;multiple myspace&rdquo;, &ldquo;multiple picasaweb&rdquo;, &ldquo;multiple twitter&rdquo; and &ldquo;multiple youtube&rdquo;. So, basically, every social network out there!<font></p>

<p><font size="2">Out of these tables I chose Google, Youtube, Facebook, &nbsp;Twitter and Blogger. The great thing about this dataset is that it&rsquo;s versatile and contains truly a lot of data - but <strong>for this study the more superficial data points such as music, books, hobbies, films, emails and links</strong>, which are tailored to the person specifically, <strong>are omitted</strong>.<br />
<br />
The fields interesting for the analysis are:</p>

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

<p>There is also last activity online.</font></p>
