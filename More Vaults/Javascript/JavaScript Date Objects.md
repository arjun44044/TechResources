 The **Date object** is an inbuilt datatype of JavaScript language. It is used to work with _dates_ and _times_. The Date object is created by using _new_ keyword, i.e. _**new Date()**_. The Date object can be used date and time in terms of **millisecond** precision within 100 million days before or after 1/1/1970.

By default, JavaScript will use the browser's time zone and display a date as a full text string:
**Tue Jan 09 2024 12:09:14 GMT+0530 (India Standard Time)**
		new Date() without arguments, creates a date object with the current date and time:
		Eg- `const d = new Date();`
				 `document.getElementById("demo").innerHTML = d;`
				O/P--Tue Jan 09 2024 12:30:09 GMT+0530 (India Standard Time)
		 new Date() with arguements creates date out of it--
		 Eg- `const d = new Date("2022-03-25");`
`document.getElementById("demo").innerHTML = d;`
				O/P--Fri Mar 25 2022 05:30:00 GMT+0530 (India Standard Time) 
--<span style="color:#c00000">There are **9 ways** to create a new date object:
</span>
new Date()  
new Date(_date string_)  
  
new Date(_year,month_)  
new Date(_year,month,day_)  
new Date(_year,month,day,hours_)  
new Date(_year,month,day,hours,minutes_)  
new Date(_year,month,day,hours,minutes,seconds_)  
new Date(_year,month,day,hours,minutes,seconds,ms_)  
  
new Date(_milliseconds_)  

--==JavaScript counts months from **0** to **11**:==

--7 numbers specify year, month, day, hour, minute, second, and millisecond (in that order):
`const d = new Date(2018, 11, 24, 10, 33, 30, 0);`
--Specifying a month higher than 11, will not result in an error but add the overflow to the next year:
   Eg- const d = new Date(2018, 15, 24, 10, 33, 30); 
     same as----> const d = new Date(2018, 6, 5, 10, 33, 30);

---
<span style="color:#c00000"><span style="color:#7030a0"> ## IMP DATE METHODS</span></span>
 <span style="color:#c00000"><span style="color:#c00000"></span></span>
 1 <span style="color:#c00000">**toString()**</span> method. This <span style="color:#c00000"><span style="color:#c00000"><span style="color:#c00000"><span style="color:#c00000"></span></span></span></span>is a string representation of the date, including the time zone.
 2 <span style="color:#c00000">toDateString()</span>  method converts a date to a more readable format
 3** <span style="color:#c00000">toUTCString()</span> ** method converts a date to a string using the UTC standard: eg--> Tue, 09 Jan 2024 06:56:32 GMT
 4 <span style="color:#c00000">toISOString() </span>method converts a date to a string using the ISO standard: eg--> 2024-01-09T06:56:35.304Z
 5  <span style="color:#c00000">Date.now()</span>` method in JavaScript returns the current timestamp in milliseconds

|Method|Description|
|---|---|
|**[Date()](https://www.geeksforgeeks.org/javascript-date-now/)**|It returns presents day’s date and time.|
|**[getDate()](https://www.geeksforgeeks.org/javascript-date-getdate-function/)**|It returns the day for the specified date.|
|**[getDay()](https://www.geeksforgeeks.org/javascript-date-getday-method/)**|It returns the day of the week for the specified date.|
|**[getFullYear()](https://www.geeksforgeeks.org/javascript-date-getfullyear-function/)**|It returns the year of the specified date.|
|**getYear()**|This method returns the year in the specified date.|
|**[getHours()](https://www.geeksforgeeks.org/javascript-date-gethours-function/)**|It returns the hour in a specified date.|
|**[getMilliseconds()](https://www.geeksforgeeks.org/javascript-date-getmilliseconds-function/)**|It returns the milliseconds in the specified date.|
|**[getMinutes()](https://www.geeksforgeeks.org/javascript-date-getminutes-method/)**|It returns the minutes in the specified date.|
|**[getMonth()](https://www.geeksforgeeks.org/javascript-date-getmonth-method/)**|It returns the month in the specified date. This also find the month.|
|**[getSeconds()](https://www.geeksforgeeks.org/javascript-date-getseconds-method/)**|This method returns the seconds in the specified date.|
|**[getTime()](https://www.geeksforgeeks.org/javascript-gettime-method/)**|This method returns the date in terms of numeric value as milliseconds.|
|**[setDate()](https://www.geeksforgeeks.org/javascript-date-setdate-function/)**|This method sets the day of the month for a specified date.|
|**[setFullYear()](https://www.geeksforgeeks.org/javascript-date-setfullyear-function/)**|This method sets the full year for a specified date.|
