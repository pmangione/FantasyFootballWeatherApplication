# FantasyFootballWeatherApplication
<b>A website that allows users to look up Fantasy Football Stats specific to weather conditions.</b>

<b><h1>DATABASE DESIGN</h1></b>

I used the ASP.NET MVC Code First for the construction of my SQL Server Database. <a href="https://github.com/pmangione/FantasyFootballWeatherApplication/blob/master/DBDiagram.PNG">VIEW DATABASE DIAGRAM HERE</a> 

The Game model contains details about each specific game such as date, temperature, and windspeed.  

The QBGame model contains a quarterback's statistics for each game that they played in such as touchdown passes and passing yards.  The QBPlayer model contains information specific to each player such as first and last name. 

Why does it look like there are 2 primary keys for each model? I import this data from a CSV file which I purchased from an outside vendor, and they have their own nomenclature.  For example, they use "ScoreID" as a unique ID for each game, while I use "GameID."  Because I may import more data from the vendor in the future and some of their field names may change, it provides the greatest flexibility if I use the their nomenclature.  

So why do I have have a GameID field? This is actually my primary key.  Initially, I tried to use ScoreID as the primary key but this tended to cause errors during data migrations.  The system seemed more stable when it was allowed to auto-generate its own primary key value in GameID rather than using the value from ScoredID in the vendor's CSV file. 

I realize this is not the ideal way to build a database, but because I needed to launch this quickly it became the best option in terms of time-saved and flexibility.  If this database becomes bigger and more complex or I decided to add sensitive data (like user information), I will reconstruct the database with more rigorous standards.   
