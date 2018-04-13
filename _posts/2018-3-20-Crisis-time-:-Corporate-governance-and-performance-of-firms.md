
 ---
 <font size = "2">
<p>This post is rightfully the first as it&rsquo;s about my master thesis paper, which is the study of corporate governance and how it affected performance of firms during the world financial crisis of 2007-2008, with a focus on emerging markets. &nbsp;</p>



<p>World financial crisis being an external shock, all firms struggled to maintain their financial performance. The overall aim was to look at corporate governance on firm level and country level and see</p>

<ul>
	<li>
	<p>whether it has any effect on the prices of stocks during the crisis</p>
	</li>
	<li>
	<p>Whether firm is better &ldquo;on its own&rdquo; or CG policies also help it survive</p>
	</li>
</ul>
<p>
This research was based on <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2177841">this</a> paper by Ruben Enikopolov, Maria Petrova and my then scientific supervisor, Sergey Stepanov.</p> </font>

<h3><strong>Corporate governance - what is it exactly?</strong></h3>
 <font size = "2">
<p>Many people seem to think that corporate governance is corporate culture, which is not the case. Academically it&rsquo;s defined as &ldquo;means of protecting the minor investors and ensuring they get a ROI&rdquo;. &nbsp;The three most prominent agencies measuring corporate governance all over the world are ISS, CLSA and Standard &amp; Poor&rsquo;s.<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<img alt="RÃ©sultat de recherche d'images pour &quot;institutional shareholder services&quot;" src="https://www.issgovernance.com/file/2014/09/Header-Logo.png" />&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<img src="https://lh5.googleusercontent.com/wBhkDWBr4Zu1WR9d0qtApr-XsVW4VgoiwMSnqtobiswguuJPrt94DrVUVTk7Hzdx0bghg5_noSRK9YzZ86cSMqA5pqp3WTv0fYwH7Ur1xBDQD1265gPbr0aZEqOXEjN4cNRh3TUw" style="height:52px; width:110px" />&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img src="https://lh5.googleusercontent.com/cF0ueamssyyqSicT3wnx5_0_lrRqCU35064WURcMaQRPLV1-iw0RmSsjGWp2RhvQjGmMtpYUCaX7rRR0lCfJ3-u8dN637cTyQ_CEMJ1u7tkm7j_TcW2zyxOMhs8x2OyLUahFbfWa" style="height:66px; width:132px" /><br />
Below is the table comparing their scores and methodologies. They&rsquo;re essentially questionnaires on a variety of subjects, upon which later the score is given. &nbsp;The one used by me was CLSA.&nbsp;</p></font>

<table border="1" cellpadding="1" cellspacing="1" style="width:1000px">
	<tbody>
		<tr>
			<td><font size = "2">ISS Governance Quick Score</font></td>
			<td><font size = "2">4100 companies, 25 countries</font></td>
			<td><font size = "2">Board structure and policies,&nbsp;Compensation and remuneration,&nbsp;Shareholder rights,&nbsp;Audit practices</font>
</td>
		</tr>
		<tr>
			<td><font size = "2">CLSA Governance Score</font></td>
			<td><font size = "2">495 companies, 25 countries</font></td>
			<td><font size = "2">Discipline, Transparency, Independency, Accountability, 
				Responsibility, Fairness, Social awareness</font></td>
		</tr>
		<tr>
			<td><font size = "2">S &amp; P CGS</font></td>
			<td>&nbsp;</td>
			<td><font size = "2">Ownership Structure &amp; Influence, Financial Stakeholder Rights and Relations, 
				Financial Transparency and Information Disclosure, Board Structure and Process</font></td>
		</tr>
	</tbody>
</table>

<p><h3><strong>The data </strong></h3>
<font size = "2">
All financial data was taken from Yahoo Finance and Thomson Reuters Datastream. The companies under examination were public, as only those have stocks trading at international exchange, and their financial information is available.<br />
	<br />
The countries taken into account were emerging markets, as on these the effect is more evident and less &ldquo;smoothed out&rdquo; or outweighed by other variables (as in Russia, for example, by corruption which makes measuring corporate governance extremely difficult). The ones under analysis were China, Hing Kong, India, Indonesia, Korea, Malaysia, Philippines, Singapore, Taiwan, Thailand, EEMEA region and Latin America region.<br />	
<br />
Back then I did all the analysis and modeling in Eviews/Excel, and here is what one of my first results looked like:</p>
</font>
<font size="2">
<h2>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img alt="" src="https://ibb.co/g82sNS" /><img alt="" src="https://ibb.co/g82sNS" /><img alt="" src="https://raw.githubusercontent.com/xenia-sh/xenia-sh.github.io/master/images/third one for corporate gov.png" style="height:396px; width:640px" />&nbsp;</h2>

<h2><strong>The model - variables &nbsp;</strong></h2>

<p>The &ldquo;window&rdquo; of time used for crisis was from June 2007 to December 2009. Therefore, The dependent variable was &nbsp;the daily closing stock price at the lowest point in this period with value at the beginning of the crisis normalized at 100. All of the other variables were taken from strictly before the crisis (2006).<br />
	<br />
There were a few groups&nbsp;of variables:<br />
-&nbsp;Corporate governance related, firm level (CLSA index and its components)<br />
-&nbsp;Corporate governance related, country level (taken from research articles - rule of law, corruption, ownership concentration, government effectiveness)<br />
-&nbsp;Performance related, country level<br />
-&nbsp;Controlling (0 or 1 depending on industry and country)<br />
-[Performance related, firm level (ROE, ROI, Net growth, D/E ratio - wasn&rsquo;t used for Python modelisation)]</p>
<br />
<h2><strong>The model - variable correlations </strong></h2>

<p>--code for correlations and results with short explanations what to exclude</p>

<h2><strong>The model - specification </strong></h2>

<p>The model specifications (multivariate regression) used in the research were:<br />
	<br />
<em>Min_share_price</em><em> = c + c1* (</em><em>CG_index</em><em>)+ c2*(</em><em>firm-level control variables</em><em>) + c3* (country-level control variables) + c4*(industry fixed effects*) + c5*(country fixed effects*)</em><br />
	<br />
<em>Min_share_price</em><em> = c + c1* (</em><em>Country variables</em><em>) + c2*(</em><em>firm-level control variables</em><em>) &nbsp;+ c3*(</em><em>country-level control variables</em><em>) + c4*(industry fixed effects*) + c5* (country fixed effects*) </em><br />
<br />
The one used by me is the combination of the two:<br />

<em>-code of predicted and predictive and of prediction + results </em></p>

<p>A few things can be immediately seen from the results.<br />
	<br />
First, Fairness (which is the component of CLSA) is significant (in fact, it&rsquo;s the only significant element of the index across multiple model specifications that I&rsquo;ve tried).<br />
Second, country dummies and industry dummies &ldquo;take away&rdquo; significance from both CLSA firm-level indicators and country parameters, being very strongly significant at 1% confidence level.<br />
Third, country level variables - GDP, Market capitalization and, especially, ownership concentration (describing the owners rights established by country law) are all significant.</p>

<h2><strong>General results &nbsp;</strong></h2>

<p>The more &ldquo;official&rdquo; output of the paper is:<br />
-&nbsp;There is weak evidence that in countries with better institutions, the effect of firm-level corporate governance is actually stronger<br />
- Complementarity of firm-level and country-level firm characteristics<br />
- Firms do rely on their own performance to save them in troubled times (which makes a lot of sense)<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img alt="" src="https://raw.githubusercontent.com/xenia-sh/xenia-sh.github.io/master/images/data science second graph from thesis pres.png" /><br />
When I presented this paper, I remember being asked about the quality of the data and it being criticized as my main limit to the strength of the model. While results were in line with previous research, certain components of CLSA index were not at all significant, and it remains a questions whether it&rsquo;s because of the sample bias or indeed they did not influence firm performance during the crisis.</font></p>
