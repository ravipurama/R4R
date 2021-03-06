<img src = /Images/R4R_header.png>

## Dates As Data
 
 | Overview | Description |
| --- | --- |
| Teaching | 10 minutes |
| Exercises | 3 minutes | 
| Question| What are good approaches for handling dates in spreadsheets? |
| Objectives | Describe how dates are stored and formatted in spreadsheets. |
||  Describe the advantages of alternative date formatting in spreadsheets. | 
|| Demonstrate best practices for entering dates in spreadsheets.|
| Presenters | [Gitanjali Yadav](http://www.nipgr.res.in/research/dr_gyadav.php) and [Ashley Sawle](https://www.cruk.cam.ac.uk/author/ashley-sawle) | 

###### Dates in spreadsheets are stored in a single column. While this seems the most natural way to record dates, it actually is not best practice. A spreadsheet application will display the dates in a seemingly correct way (to a human observer) but how it actually handles and stores the dates may be problematic.

###### In particular, please remember that functions that are valid for a given spreadsheet program (be it LibreOffice, Microsoft Excel, OpenOffice, Gnumeric, etc.) are usually guaranteed to be compatible only within the same family of products. If you will later need to export the data and need to conserve the timestamps, you are better off handling them using one of the solutions discussed below.

###### Additionally, Excel can <a href="https://nsaunders.wordpress.com/2012/10/22/gene-name-errors-and-excel-lessons-not-learned/">turn things that aren’t dates into dates</a>, for example names or identifiers like MAR1, DEC1, OCT4. So if you’re avoiding the date format overall, it’s easier to identify these issues


---
## Exercise 1

### Challenge: pulling month, day and year out of dates</p>

  * In the <code class="highlighter-rouge">dates</code> tab of your spreadsheet you have the data from 2014 plot 3. 
There’s a <code class="highlighter-rouge">Date collected</code> column.</li>
  * Let’s extract month, day and year from the dates to new columns. For this we  can use the built in Excel functions <code class="highlighter-rouge">YEAR()</code> <br><code class="highlighter-rouge">MONTH()</code>  <br />
<code class="highlighter-rouge">DAY()</code>
  * (Make sure the new column is formatted as a number and not as a date.)
  * You can see that even though you wanted the year to be 2014, your spreadsheet program automatically interpreted it as 2015, the year you entered the data.</p>

---
## Exercise 2

### Challenge: pulling hour, minute and second out of the current time</p>

Current time and date are best retrieved using the functions <code class="highlighter-rouge">NOW()</code>, which
returns the current date and time, and <code class="highlighter-rouge">TODAY()</code>, which returns the current
date. The results will be formatted according to your computer’s settings.</p>

1) Extract the year, month and day from the current date and time string returned by the <code class="highlighter-rouge">NOW()</code> function.
2) Calculate the current time using <code class="highlighter-rouge">NOW()-TODAY()</code>
3) Extract the hour, minute and second from the current time using functions <code class="highlighter-rouge">HOUR()</code>, <code class="highlighter-rouge">MINUTE()</code> and <code class="highlighter-rouge">SECOND()</code>.
4) Press <code class="highlighter-rouge">F9</code> to force the spreadsheet to recalculate the <code class="highlighter-rouge">NOW()</code> function, and check that it has been updated.

<blockquote>
  <p>Note: Excel is unable to parse dates from before 1899-12-31, and will thus leave these untouched.  If you’re mixing historic data
from before and after this date, Excel will translate only the post-1900 dates into its internal format, thus resulting in mixed data.
If you’re working with historic data, be extremely careful with your dates!</p>
</blockquote>

<p>Excel also entertains a second date system, the 1904 date system, as the default in Excel for Macintosh. This system will assign a
different serial number than the <a href="https://support.microsoft.com/en-us/help/214330/differences-between-the-1900-and-the-1904-date-system-in-excel">1900 date system</a>. Because of this,
<a href="http://uc3.cdlib.org/2014/04/09/abandon-all-hope-ye-who-enter-dates-in-excel/">dates must be checked for accuracy when exporting data from Excel</a> (look for dates that are ~4 years off).</p>

<h2 id="data-formats-in-spreadsheets">Data formats in spreadsheets</h2>

<p>Spreadsheet programs have numerous “useful features” which allow them to handle dates in a variety of ways.</p>

<p><img src="/Images/5_excel_dates_1.jpg" alt="Many formats, many ambiguities" /></p>

<p>But these “features” often allow ambiguity to creep into your data. Ideally, data should be as unambiguous as possible.</p>

<h3 id="dates-stored-as-integers">Dates stored as integers</h3>

<p>The first thing you need to know is that Excel stores dates as numbers - see the last column in the above figure. Essentially, it counts the days from a default of December 31, 1899, and thus stores July 2, 2014 as  the serial number 41822.</p>

<p>(But wait. That’s the default on my version of Excel. We’ll get into how this can introduce problems down the line later in this lesson. )</p>

<p>This serial number thing can actually be useful in some circumstances. By using
the above functions we can easily add days, months or years to a given date.
Say you had a sampling plan where you needed to sample every thirty seven days.
In another cell, you could type:</p>

<div class="highlighter-rouge"><div class="highlight"><pre class="highlight"><code>=B2+37
</code></pre></div></div>

<p>And it would return</p>

<div class="highlighter-rouge"><div class="highlight"><pre class="highlight"><code>8-Aug
</code></pre></div></div>

<p>because it understands the date as a number <code class="highlighter-rouge">41822</code>, and <code class="highlighter-rouge">41822 + 37 = 41859</code>
which Excel interprets as August 8, 2014. It retains the format (for the most
part) of the cell that is being operated upon, (unless you did some sort of
formatting to the cell before, and then all bets are off). Month and year
rollovers are internally tracked and applied.</p>

<p><strong>Note</strong>
Adding years and months and days is slightly trickier because we need to make
sure that we are adding the amount to the correct entity.</p>

<ul>
  <li>First we extract the single entities (day, month or year)</li>
  <li>We can then add values to to that</li>
  <li>Finally the complete date string is reconstructed using the <code class="highlighter-rouge">DATE()</code> function.</li>
</ul>

<p>As for dates, times are handled in a similar way; seconds can be directly
added but to add hour and minutes we need to make sure that we are adding
the quantities to the correct entities.</p>

<p>Which brings us to the many different ways Excel provides in how it displays dates. If you refer to the figure above, you’ll see that
there are many ways that ambiguity creeps into your data depending on the format you chose when you enter your data, and if you’re not
fully aware of which format you’re using, you can end up actually entering your data in a way that Excel will badly misinterpret.
	
---

## Exercise 3

* What happens to the dates in the “dates” tab of our workbook if we save this sheet in Excel (in <code class="highlighter-rouge">csv</code> format) and then open the file in a plain text editor (like TextEdit or Notepad)? 
* What happens to the dates if we then open the <code class="highlighter-rouge">csv</code> file in Excel?

---

<p><strong>Note</strong><br />
You will notice that when exporting into a text-based format (such as CSV), Excel will export its internal date integer instead of a useful value (that is, the dates will be represented as integer numbers). This can potentially lead to problems if you use other software to manipulate the file.</p>

<h3 id="advantages-of-alternative-date-formatting">Advantages of Alternative Date Formatting</h3>

<h3 id="-storing-dates-as-year-month-day"><a name="day"></a> Storing dates as YEAR, MONTH, DAY</h3>

<p>Storing dates in YEAR, MONTH, DAY format helps remove this ambiguity. Let’s look at this issue a bit closer.</p>

<p>For instance this is a spreadsheet representing insect counts that were taken every few days over the summer, and things went something like this:</p>

<p><img src="/Images/6_excel_dates_2.jpg" alt="So, so ambiguous, it's even confusing Excel" /></p>

<p>If Excel was to be believed, this person had been collecting bugs <strong>in the future</strong>. Now, we have no doubt this person is highly capable,
but I believe time travel was beyond even their grasp.</p>

<p>Entering dates in one cell is helpful but due to the fact that the spreadsheet programs may interpret and save the data in different ways
(doing that somewhat behind the scenes), there is a better practice.</p>

<p>In dealing with dates in spreadsheets, separate date data into separate fields (day, month, year), which will eliminate any chance of
ambiguity.</p>

<h3 id="-storing-dates-as-year-day-of-year"><a name="doy"></a> Storing dates as YEAR, DAY-OF-YEAR</h3>

<p>There is also another option. You can also store dates as year and day of year (DOY). Why? Because depending on your
question, this might be what’s useful to you, and there is practically no possibility for ambiguity creeping in.</p>

<p>Statistical models often incorporate year as a factor, or a categorical variable, rather than a numeric variable, to account for 
year-to-year variation, and DOY can be used to measure the passage of time within a year.</p>

<p>So, can you convert all your dates into DOY format? Well, in Excel, here’s a useful guide:</p>

<p><img src="/Images/7_excel_dates_3.jpg" alt="Kill that ambiguity before it bites you!" /></p>

<h3 id="-storing-dates-as-a-single-string"><a name="str"></a> Storing dates as a single string</h3>

<p>Another alternative could be to convert the date string
into a single string using the <code class="highlighter-rouge">YYYYMMDDhhmmss</code> format.
For example the date <code class="highlighter-rouge">March 24, 2015 17:25:35</code> would
become <code class="highlighter-rouge">20150324172535</code>, where:</p>

<p>YYYY:   the full year, i.e. 2015<br />
MM:     the month, i.e. 03<br />
DD:     the day of month, i.e. 24<br />
hh:     hour of day, i.e. 17<br />
mm:     minutes, i.e. 25<br />
ss:     seconds, i.e. 35</p>

<p>Such strings will be correctly sorted in ascending or descending order, and by
knowing the format they can then be correctly processed by the receiving
software.</p>




 

## Key Points
- Treating dates as multiple pieces of data rather than one makes them easier to handle.
---

| <a href="/Documents/Day02_Problems.md"><span class="glyphicon glyphicon-menu-left" aria-hidden="true"></span><span class="sr-only">Back To Previous Lesson</span></a> | <a href="/Documents/Day03.md"><span class="glyphicon glyphicon-menu-right" aria-hidden="true"></span><span class="sr-only">Go To Next Lesson</span></a> | <a href="/Documents/Day02_Sol2.md"><span class="glyphicon glyphicon-menu-right" aria-hidden="true"></span><span class="sr-only">Solutions To Exercises</span></a> 
  | ---- | ----|---- | 
  
---
Licensed under <a href="">CC-BY 4.0</a> 2018–2019 by <a href="https://carpentries.org/">The Carpentries</a>
        <br>
Licensed under <a href="">CC-BY 4.0</a> 2016–2018 by <a href="http://datacarpentry.org">Data Carpentry</a>
	
