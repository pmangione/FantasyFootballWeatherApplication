# FantasyFootballWeatherApplication
<b>I built a website that allows users to look up Fantasy Football Stats specific to weather conditions.</b>
<b>https://www.overunderfantasyweather.com/</b>
<br><br><b>While I am not allowed to post the entire code repository for this application, here are some highlights and code snippets.</b>

<b><h1><a id="DBDesign">YOUTUBE VIDEO SERIES SHOWING WEBSITE DESIGN, CODING HIGHLIGHTS, AND CLEAN CODE APPROACH</a></h1></b>

I enjoy talking about code almost as much as I enjoy writing code.  That is why I made 18 YouTube videos disucssing how I coded certain sections of this website.  These videos are divided into 2 series which are listed below.

<b>My Coding Style</b> - A 9 part video series showing the advantage of using long and descriptive variable names combined with short methods. 

<a href="https://www.youtube.com/watch?v=ia_3N-EiekQ">Part 1: An Introduction to My Coding Style</a>

<a href="https://www.youtube.com/watch?v=mWpimqtJt4Y">Part 2: Naming 2 Important ViewModels</a>

<a href="https://www.youtube.com/watch?v=xM_DcRNaBRw&t=153s">Part 3: Create Helper Method to Query Database</a>

<a href="https://www.youtube.com/watch?v=JtLzyGSdn_Q">Part 4: Pass Query Records To a List of ViewModels</a>

<a href="https://www.youtube.com/watch?v=8TEbi1wLJC4">Part 5: Filter List of ViewModels Based on Weather Conditions</a>

<a href="https://www.youtube.com/watch?v=4POUkZv1bQI">Part 6: Performing Calculations and Using Inheritance in ViewModels</a>

<a href="https://www.youtube.com/watch?v=d3F4iwkJ_ac">Part 7: Using Polymorphism With ViewModels</a>

<a href="https://www.youtube.com/watch?v=HUEHPfVxcH4">Part 8: Why ViewModel Includes Certain Fields</a>

<a href="https://www.youtube.com/watch?v=1KTsrCnfeq0">Part 9: Final Summary of My Coding Style</a>



<b>Website Overview, Database Structure, and Using ViewModels</b> - A 9 part video series showing basic website functionality, database design, and the process of model binding with forms and ViewModels.

<a href="https://www.youtube.com/watch?v=IiDshJ2hZ20">Part 1: How the Website Works</a>

<a href="https://www.youtube.com/watch?v=5ulDdM2po0A&t=6s">Part 2: Database Structure</a>

<a href="https://www.youtube.com/watch?v=aUBeMpf8d_w">Part 3: Example of Database Record</a>

<a href="https://www.youtube.com/watch?v=Duptc5TROWU">Part 4: Database Code</a>

<a href="https://www.youtube.com/watch?v=urB3gbKbI0k&t=2s">Part 5: Example of ViewModel</a>

<a href="https://www.youtube.com/watch?v=ul8jzQemPOs">Part 6: How ViewModel Works On Website</a>

<a href="https://www.youtube.com/watch?v=cV8_IqvqzeU">Part 7: Building ViewModel In Controller</a>

<a href="https://www.youtube.com/watch?v=5AKC4vLtaPw">Part 8: Binding ViewModel Data in View</a>

<a href="https://www.youtube.com/watch?v=tOgIONB5Aoc">Part 9: Retrieving ViewModel Data in Controller</a>


<b><h1><a id="DBDesign">DATABASE DESIGN</a></h1></b>

I used ASP.NET MVC Code First for the construction of the SQL Server Database. <a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/DBDiagram.PNG">VIEW DATABASE DIAGRAM HERE</a> 

The Game model contains details about each specific game such as date, temperature, and windspeed.  

The QBGame model contains a quarterback's statistics for each game that they played in such as touchdown passes and passing yards.  The QBPlayer model contains information specific to each player such as first and last name. 

Why does it look like there are 2 primary keys for each model? I import this data from a CSV file which I purchased from an outside vendor, and they have their own nomenclature.  For example, they use "ScoreID" as a unique ID for each game, while I use "GameID."  Because I may import more data from the vendor in the future and some of their field names may change, it provides the greatest flexibility if I use the their nomenclature.  

So why do I have have a GameID field? This is actually my primary key.  Initially, I tried to use ScoreID as the primary key but this tended to cause errors during data migrations.  The system seemed more stable when it was allowed to auto-generate its own primary key value in GameID rather than using the value from ScoredID in the vendor's CSV file. 

I realize this is not the ideal way to build a database, but because I needed to launch this quickly it became the best option in terms of time-saved and flexibility.  If this database becomes bigger and more complex or I decided to add sensitive data (like user information), I will reconstruct the database with more rigorous standards.   

<b><h1><a id="ModelBinding">MODEL BINDING</a></h1></b>

I used model binding to pass many of my values from user entry forms to the controller.  For the sake of simplicity, let's follow one value through this process. The value relates to wind speed and is the dropdown on the user-entry form in which the user can choose "Greater Or Equal Than" OR "Less Or Equal Than". 

1) <a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/UserEntryFormWebsiteScreenShot.PNG">  This is the what the user-entry form looks like on the website</a>.  I have circled the windspeed greater-or-equal than dropdown.

2) <a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/BindingExampleOnUserEntryForm.JPG"> This is a code snippet from the "View" for the user entry form.</a>  I have circled the "WindSpeedGreaterOrLessThan" property which is bound to a ViewModel object. 

3) <a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/BindingExampleInController.JPG"> This is the code for the part of the "Controller" that receives the "WindSpeedGreaterOrLessThan" value.</a> The "qbuserchoosesweatherspecifics" object is passed in as a parameter for the ShowWeatherSummaryStats method of the controller (I have circled this at the top of the code).  The value is bound to the WinidSpeedGreaterOrLessThan property of the “qbuserchoosesweatherspecifics" object. The WindSpeedGreaterOrLessThan value can then be used in other parts of the code.   For example, the WindSpeedGreaterOrLessThan value is passed as a parameter to a method that creates a list of games that the quarterback has played under specific weather conditions (this is circled near the bottom of the code).

So why use model binding and not the FormCollection to pass the WindSpeedGreaterOrLessThan value to the controller? There are many reasons why I did not want to use FormCollection. To me, the most important reason is my goal to avoid retrieving values in the controller that are not strongly typed to the “qbuserchoosesweatherspecifics” model. For example, I might misspell "WindSpeedGreaterOrLessThan" as "WindSpeedGreaterOrLessThEn" when trying to pull the value from FormCollection. The intellisense in Visual Studio would not alert me to this error and I might end up running the code and waste time trouble-shooting the issue. However, with model binding, the intelliSense will alert me to this error.  In general, working with strongly typed variables is preferable since it reduces the chance of unexpected errors. 



<b><h1><a id="HelperMethods">HELPER METHODS</h1></b>

For this project, I created many "Helper Methods" for several reasons:
1) Reusability
2) Code Readability

<a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/CallingHelperMethodExampleOne.JPG"> This is an example of one of the Helper Methods in the Controller.</a>  I have circled a line of code where the method, called "makeQBVMGameListForSpecificQB", is used.   In this example, it’s used to create a list of games played by the quarterback that is then passed to a method which filters out ONLY games played in specific weather.
<a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/CallingHelperMethodExampleTwo.JPG"> Here is an example of how this same Helper Method is used in a different part of the controller. </a>  In this example, it’s used to create a list of games played by the quarterback for games in all types of weather.
Because the makeQBVMGameListForSpecificQB method is used in several lines of code, the code is more readable if it is abstracted away. Moreover, if a 2nd developer came into the project and needed change the way in which the resulting list from the method was handled, they could clearly see where to go in the code. In addition, if changes were needed for the makeQBVMGameListForSpecificQB itself, the changes would only need to be made in one place.

<a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/HelperMethodMakeQBVmGameList.PNG"> This is the actual makeQBVMGameListForSpecificQB method.</a>  Note for the sake of readability and reusability, there are several helper methods within this method that are also abstracted away.  Also note the use of the LINQ operator "OrderByDescending" for the GameDate.  This is used so that the website user can view the most recent games first.    

<a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/ScreenShotGamesByQB.PNG"> This is an example of the list of games ordered by date in descending order.</a>



