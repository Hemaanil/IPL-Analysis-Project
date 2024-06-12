## IPL ANALYSIS -> 2008 - 2022

## Key Insights -
1-	Find the Title winner, Orange Cap Winner, Purple Cap Winner, Tournament 6’s & 4’s for the respective seasons on IPL.

2-	Develop IPL Bating and Bowling stats and add a filter where user can select the bowler and batsman to see these stats.

3-	Winning percentage based on the toss decision

4-	Matches win by venue

5-	Total wins by team in a season

6-	Matches won based on result type.

## Process - 
- Step 1 : 	We have one data set name of IPL matches of 2008-2022
(in this id column is primary key where all values are unique)
Another data set is ipl ball by ball of 2008-2022
(in this id column is foreign key where many values are duplicate)

- Step 2 : Now lets import the above 2 files in postgre sql (pg admin) to make it as SQL data set.
Next create database in pg  admin with the name of IPL database.
(As it is a new database we dnt have tables, so now we have to create tables which we have gone through previous.)
Now we will create table in the name of ipl matches 2008-2022by writing query-----right click of the table (ipl database) query option.
Query – 
## CREATE TABLE ipl_matches_2008_2022

(
    id int8 PRIMARY KEY,

    city varchar(50),

    match_date date,

    season varchar(50),

    match_number varchar(50),

    team1 varchar(50),

    team2 varchar(50),

    venue varchar(100),

    toss_winner varchar(50),

    toss_decision varchar(50),

    superover varchar(50),

    winning_team varchar(50),

    won_by varchar(50),

    margin varchar(50),

    method varchar(50),

    player_of_match varchar(50),

    umpire1 varchar(50),

    umpire2 varchar(50)
)

- Step 3 : Now RUN the query, table will create
 see wether the table is created or not  just in query platform in another line write a simple query of 

SELECT 
* 
FROM ipl_matches_2008_2022

- Step 4 : Now u can see the table is created in pg admin with out data in it…..only table with columns  with data types u can see.
Now next step is to add data in to  the table .
Here we have to write the statement like ----- 
# COPY ipl_matches_2008_2022 FROM 'D:\ipl_matches_2008_2022.csv' DELIMITER ',' CSV HEADER;
*Note- 'D:\ipl_matches_2008_2022.csv' 🡪 this is the location path of the file where it is placed, we need to import this file data in our table ….
Here DELIMETER is nothing but CSV  file or comma separated files after this we have to enter the datatype or have to give the file extension  it is CSV.
And HEADER is nothing but this particular file already have headers
Now RUN the command.
- Step 5  : To see the data uploaded in the created table run the select command again.
Now u can see the ipl datable in TABLES, if u cant see the created table refresh the TABLE.
Next step is create another table in the same way.
Now we will create table in the name of ipl_ball_by_ball_ 2008-2022by writing query-----right click of the table (ipl database) query option.
Query –

CREATE TABLE ipl_ball_by_ball_2008_2022

(
id int8 NOT NULL,

innings int8,

overs int8,

ball_number int8,

batter varchar(50),

bowler varchar(50),

non_striker varchar(50),

extra_type varchar(50),

batsman_run int8,

extras_run int8,

total_run int8,

non_boundary int8,

iswicket_delivery int8,

player_out varchar(50),

dismisal_kind varchar(50),

fielders_involved varchar(50),

batting_team varchar(50))

and remaining process everything will be same like above table.
Now open the POWERBI and get the data from postgre SQL database.
Give server name as LOCALHOST and  database as ipl_database and import-ok.

Step 6 : Check the relationship status(cardinality) in model view. It should be connected, if not do it manually by matching primary key to foreign key.
We can see 1 to many relationship.
Now check for data cleansing for ipl_matches_2008_2022
Go to powery query editor

1-	From team1 column need to do small changes. Open that column filter, deselect all, and select only  Rising pune super gaint and Rising pune gaints.go to transform ribbon – replace values with rising pune supergaints. Now remove filtered rows in applied steps, so that filter will be removed and we will get normal data with changes.

2-	Same changes should be done to team2 column.

3-	Changes in city column for Bangalore to Bengaluru.

4-	Home – close n apply.

# Data Processing
1-	Create Date Table

2-	Table tools -  Create Table – now give the 
Calendar funcn

*Calendar Table = CALENDAR(MIN(ipl_matches_2008_2022[match_date]),MAX(ipl_matches_2008_2022[match_date]))*

![Screenshot 2024-06-13 001201](https://github.com/Hemaanil/IPL-Analysis-Project/assets/165702332/392cbd64-f5c5-4f8c-9e42-ebff6f621aea)


3-	Extract the year frm the date table.

New column – write the function

Year = YEAR('Calendar Table'[Date])

4-	Go to model view, create the relation ship b/w date table and match date table.

# Time to Generate Reports-

1-First change the page background colour
Report view – format options – canvas background – image(browse).
Now do the settings – reduce the transparency in canvas background from 100% to 0%, image fit – normal to fit.

2-Insert Title – go to Insert – shapes-rectangle- adjust full length
For text again go to insert – text box – name the title as IPL ANALYSIS and do the settings

# Creating Reports based on Requirements – 
1-	Title Winner  for the particular reason – 
To see the result in particular season first create slicer with the ‘year column’ in  field in calendar table . Now do the settings for the slicer. Change from between Dropdown with multiselect on in slicer settings and adjust in title bar.. Rename the slicer name from Year to Select Season.
     	Now have to find winner, for that we have to do calculation with max date in calendar table. As we have to find a single value we have to go for Measures.

*Title Winner = 
VAR max_date =
    CALCULATE(
        MAX('Calendar Table'[Date]),
        ALLSELECTED(ipl_matches_2008_2022),
        VALUES(ipl_matches_2008_2022)
    )
VAR title_winner =
    CALCULATE(
        SELECTEDVALUE(ipl_matches_2008_2022[winning_team]),
        'Calendar Table'[Date] = max_date
    )
RETURN
    title_winner*

In this measure:

•	max_date calculates the maximum date from the 'Calendar Table' considering filters from 'ipl_matches_2008_2022'.

•	title_winner calculates the winning team for the max_date from 'ipl_matches_2008_2022'.

•	The RETURN statement returns the title_winner.

We will get the result.

--> Now use KPI card visual for report. Next take rectangle make some changes and insert this kpi value in that rectangle. Take duplicates of that rectangle and kpi value (ctrl+c, ctrl+v) so that for next visuals we need not to do same changes which takes more tym. For next requirements which are in kpi s we have to deselect the fields in build settings to add new value.

![Screenshot 2024-06-13 001439](https://github.com/Hemaanil/IPL-Analysis-Project/assets/165702332/5cfee62e-376a-4b34-aabb-912ed99e0160)



--> Orange Cap Holder -  Add batter column in kpi fields from ipl_ball_by_ball_2008_2022
Also add batter column in  filter on this visual in filters next select TOP N from basic filtering and keep 1 in TOP option and add batsman_run  column in by value field and apply filter. Now ull get correct results. Now change the title to Orange Cap. Now to know the runs that particular batsman did again take one more duplicate of that kpi, deselect that field and add batsman_run column in the buld fields and insert that runs value in that rectangle box. Now to get the text RUNS beside that score, we have to use concatenate function.

![Screenshot 2024-06-13 001514](https://github.com/Hemaanil/IPL-Analysis-Project/assets/165702332/b78bd711-bfa9-4a9e-9af4-c4116a133c0c)


*Batter_Runs = CONCATENATE(SUM(ipl_ball_by_ball_2008_2022[batsman_run]), " Runs")*

--> Purple Cap Holder – Do the same process like orange cap but select bowler column in build fields, filter fields and iswicket_delivery in by value field, later in another data field add dismissal_kind column and in that filters select only bolls score, deselect n/a and runs options.
Now change the title into purple cap. Next have to find out Wickets. Same process like Runs along vth calcn.

![Screenshot 2024-06-13 001529](https://github.com/Hemaanil/IPL-Analysis-Project/assets/165702332/14b9c24d-a77a-4eba-b3ba-516c6b416d88)



*Wickets_Measure = CONCATENATE(SUM(ipl_ball_by_ball_2008_2022[iswicket_delivery]), " Wickets")*

--> Tournament 6s – Add the batsman_run in build visuals field, in that select > from that select count. Next add batsman_run in filter fields,  in that filters select basic filtering, in that select only 6. Next add non_ boundaries to the another data field, in that also select basic filtering option and in that select only 0.

![Screenshot 2024-06-13 001551](https://github.com/Hemaanil/IPL-Analysis-Project/assets/165702332/10b7ac97-eccd-46da-b5a8-c704600b3e61)


--> Tournament 4s -  Same like above changes, but instead of 6s select 4s, and change the title as Tournament4s.

![Screenshot 2024-06-13 001615](https://github.com/Hemaanil/IPL-Analysis-Project/assets/165702332/2c08394a-d2fe-4412-8d49-2d8134ec8773)


--> Bating and Bowling Stats -  As asked in requirement first we have to add slicer and add batter in the fields and make it as drop down and single select. In order to not to get disturbed to the above slicer and kpis off the edit interactions which will be in format ribbon…… Next for total runs create card visual with batsman_run in the fields and do some changes to fit that value in the rectangle and change the title to Total Runs. Now for no.of 6s and for no.of 4s we have to do the same process that how we have done in tournament 6s, 4s……. now have to do strike rate.
First of all what is strike rate – Divide total no.of runs scored in an innings by the no.of  deliveries(balls) faced by the batsman.

*Strike rate for Batsman = (SUM(ipl_ball_by_ball_2008_2022[batsman_run])/COUNT(ipl_ball_by_ball_2008_2022[ball_number]))*100**
 
--> Now select the card visual and add the strike rate for batsman measure in the fields and change the title to strike Rate.
 --> Next for bowling also same process like above, but add bowler column to the fields. So for 1st card we have to find wickets. For that we have to consider iswicket_delivery column. And in filters column add dismissal_kind column and in that filter basic filtering select some options apart of Runs. Now make the changes and insert in to rectangle and change the title in to wickets.
 --> Now we have to find Economy.------ What is economy rate?
Dividng the no.of runs conceded by a bowler by the no. of overs bowled. So we have to make calculn of 

*Economy = DIVIDE(SUMX(FILTER(ipl_ball_by_ball_2008_2022, ipl_ball_by_ball_2008_2022[extra_type]<>"legbyes"&&ipl_ball_by_ball_2008_2022[extra_type]<>"byes"), ipl_ball_by_ball_2008_2022[total_run]), (COUNT(ipl_ball_by_ball_2008_2022[overs]))/6)*

--> Next we have to find Average.------ what is meant by average of the bowler 
Dividng the no.of runs conceded by a bowler by the no. of wickets they have taken.

*Average by Bowler = DIVIDE(SUMX(FILTER(ipl_ball_by_ball_2008_2022, ipl_ball_by_ball_2008_2022[extra_type]<>"legbyes"&&ipl_ball_by_ball_2008_2022[extra_type]<>"byes"), ipl_ball_by_ball_2008_2022[total_run]), SUM(ipl_ball_by_ball_2008_2022[iswicket_delivery]))*

 After this measure change the title and insert that card in above rectangle.

--> Now we have to find bowling strike rate – Create measure

*Bowling Strike Rate = COUNT(ipl_ball_by_ball_2008_2022[bowler])/SUM(ipl_ball_by_ball_2008_2022[iswicket_delivery])*

Take card visual and add this measure column in fields and change the title in to Bowling SR and insert in to rectangle.


3-	Winning Percentage based on toss decission-  

*Matches Win on Toss Decission =CALCULATE(COUNTROWS(ipl_matches_2008_2022), ipl_matches_2008_2022[toss_winner] = ipl_matches_2008_2022[winning_team])*

Take pie chart , add measure in values and toss decision in legend fields and make the changes.

4-	Matches won by Result Type-  Take the Donut chart and add id column in values field amd won by column in legend fields and do the changes in  colours, title, etc.

5-	Matches Win By Venue -  Take Stacked Bar Chart and add venue on y-axis and Id ON X-Axis and won_by column to legend field.

6-	Total wins by team in a season -  Take same  stacked bar  chart and add winning_team on x-axis and same winning _ team on y-axis.

7-	Finally  to add Images – Go to insert select image and upload images do adjustments and insert it.

And the final Awesome Report looks like
![Screenshot 2024-06-13 001024](https://github.com/Hemaanil/IPL-Analysis-Project/assets/165702332/5fe06504-a77a-45de-81ff-a4e8d3a7c305)




