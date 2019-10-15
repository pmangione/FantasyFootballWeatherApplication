# FantasyFootballWeatherApplication
<b>A website that allows users to look up Fantasy Football Stats specific to weather conditions.</b>
<b>https://www.overunderfantasyweather.com/</b>


<b><h1>DATABASE DESIGN</h1></b>

I used the ASP.NET MVC Code First for the construction of the SQL Server Database. <a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/DBDiagram.PNG">VIEW DATABASE DIAGRAM HERE</a> 

The Game model contains details about each specific game such as date, temperature, and windspeed.  

The QBGame model contains a quarterback's statistics for each game that they played in such as touchdown passes and passing yards.  The QBPlayer model contains information specific to each player such as first and last name. 

Why does it look like there are 2 primary keys for each model? I import this data from a CSV file which I purchased from an outside vendor, and they have their own nomenclature.  For example, they use "ScoreID" as a unique ID for each game, while I use "GameID."  Because I may import more data from the vendor in the future and some of their field names may change, it provides the greatest flexibility if I use the their nomenclature.  

So why do I have have a GameID field? This is actually my primary key.  Initially, I tried to use ScoreID as the primary key but this tended to cause errors during data migrations.  The system seemed more stable when it was allowed to auto-generate its own primary key value in GameID rather than using the value from ScoredID in the vendor's CSV file. 

I realize this is not the ideal way to build a database, but because I needed to launch this quickly it became the best option in terms of time-saved and flexibility.  If this database becomes bigger and more complex or I decided to add sensitive data (like user information), I will reconstruct the database with more rigorous standards.   

<b><h1>MODEL BINDING</h1></b>

I used model binding to pass many of my values from user entry forms to the controller.  For the sake of simplicity, let's follow one value through this process. The value relates to wind speed and is the dropdown on the user-entry form in which the user can choose "Greater Or Equal Than" OR "Less Or Equal Than". 

1) <a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/UserEntryFormWebsiteScreenShot.PNG">  This is the what the user-entry form looks like on the website</a>.  I have circled the windspeed greater-or-less than dropdown.

2) <a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/QBPlayerDataModel.PNG">  This is the QBPlayer data model.</a>  The "WindspeedGreaterOrLessThan" is a "NotMapped" value because I do not want it going into the database.  It will be used to hold a temporary value of the user preference.

3) <a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/UserEntryFormCodeSnippet.PNG"> This is a code snippet from the "View" for the user entry form.</a>  I have circled the QBPlayer model reference on the top and also the line of code in which the value is bound to the "WindSpeedGreaterOrLessThan" property of the QBPlayerModel object. 

4) <a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/ControllerCodeSnippetThatReceivesFormValues.PNG"> This is the code for the part of the "Controller" that receives the "WindSpeedGreaterOrLessThan" value.</a>  The "qbplayer" object is passed in as a parameter and because the value is bound to the WindSpeedGreaterOrLessThan property of the qbplayer object.  The WindSpeedGreaterOrLessThan value is actually passed to two different places:  

A) In the long line of code that starts with "var viewmodellist....", I have highlited in yellow where this value is passed to a method used to retrieve games played under certain weather conditions.  We won't get into the specifics of how that method works in this discussion.

B) In the bottom 3 highlited lines, the WindSpeedGreaterOrLessThan is passed to another model(QBStatsSummarySpecificWeather), and that model is actually a property of another model(QBVM).  QBVM is then passed to another view with the QBStatsSummarySpecificWeather wrapped inside of it.   

So why use model binding and not the FormCollection?  There are many reasons why I did not want to use FormCollection.  To me, the most important reason is my goal to avoid retrieving values in the controller that are not strongly typed to the QBPlayer model.  For example, I might misspell "WindSpeedGreaterOrLessThan" as "WindSpeedGreaterOrLessThEn" when trying to pull the value from FormCollection.  The intellisense in Visal Studio would not alert me to this error and I might end up running the code and waste time trouble-shooting the issue.  However, with model binding, the intellisense will alert me to this error.    


<b><h1>HELPER METHODS</h1></b>

For this project, I created many "Helper Methods" for several reasons:
1) Resusability
2) Code Readability

<a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/HelperMethodsUsedInController.PNG"> This is an example of how one of the Helper Methods in used in the Controller.</a>  I have highlited several lines of code where the method, called "makeQBVMGameListForSpecificQB", is used.  QBVM is my Quarterback View Model which contains data specif to game performance of the quarterback.  

Note that in the controller, the method is used in two different places.  

In the top highited line of code, it's used to create a list of games played by the quarterback that is then passed to a method which filters out ONLY games played in specific weather.

In the bottom line of code, it's used to create a list of games played by the quarterback that is eventually passed attached to a model used by the View. 

Because the makeQBVMGameListForSpecificQB method is several lines of code, the code is more readable if it is abstracted away.  Moreover, if a 2nd developer came into the project and needed change the way in which the resulting list from the method was handled, they could clearly see where to go in the code.     In addition, if changes were needed for the makeQBVMGameListForSpecificQB itself, the changes would only need to be made in one place.

Now open the "MakeQBVMGameList" image.  This is actual method.  Note for the sake of readability and reusability, there are several helper methods within this method that are also abstracted away.  Also note the use of the LINQ operator "OrderByDescending" for the GameDate.  This is used so that the website user can view the most recent games first.    

To see an example of the list of games, open the "ScreenShotGameByQB" file. 



