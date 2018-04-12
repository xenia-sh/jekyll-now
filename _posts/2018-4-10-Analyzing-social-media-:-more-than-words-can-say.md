
<p><font size="2">First of all, this is an interesting story (and an ever more interesting dataset)<font>.
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<img src="https://lh6.googleusercontent.com/c5rXEmpnUUzvwhHicFHhVteAjGcDu-qhCpyapZ8oQOybb5kaD4PRF5xI_6_MbhAszAPTw99DE15C3qMDGGH7MJW1mzmyTjkBOU-qcSDzjXev3ln0fuvgDWatX3w8FJfXTDRiAdFa" style="height:132px; width:624px" /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br />

---
  
<font size="2">I was going through the materials to showcase in this blog and came across a presentation me and my ESCP classmates did on Microsoft. It has stock prices graphs, revenue indicators, product portfolio and many other pieces of info that we scraped across multiple public sources.<br />
First I thought about expanding on that and looking at behavior of Microsoft stocks more closely, but then, almost immediately, I had an idea - what about the people? Are people working for Microsoft any different from employees of other companies? And if they are, in what way? And this ultimately led to me searching for data of people&rsquo;s social media accounts and this study (especially since it&rsquo;s analyzing text rather than numerical values as in previous posts).<br />
<br />
Not long after I came across <a href="https://aminer.org/data-sna#Linkedin">this</a> dataset which is data on nearly 160 000 social media accounts. It&rsquo;s in MySQL Dump format, and for converting it to CSV using Python <a href="https://github.com/jamesmishra/mysqldump-to-csv">this&nbsp;tutorial</a> is quite helpful.<br />
Upon looking at what we have I&rsquo;ve found out that the tables available are &ldquo;multiple blogger&rdquo;, &ldquo;multiple facebook&rdquo;, &ldquo;multiple flickr&rdquo;, &ldquo;multiple google&rdquo;, &ldquo;multiple lastfm&rdquo;, &ldquo;multiple livejournal&rdquo; , &ldquo;multiple myspace&rdquo;, &ldquo;multiple picasaweb&rdquo;, &ldquo;multiple twitter&rdquo; and &ldquo;multiple youtube&rdquo;. So, basically, every social network out there!<font></p>
