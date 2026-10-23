Now we understand that the team can’t move between these cities on the same day to play multiple matches, so we have to modify the schedule for that week as well. The preceding verifications are intended to showcase the capabilities of pandas for data validation. Our business rule is to ensure each team gets only one match per week. Let us validate that business rule in our sample data. We will convert the start time into a date field, extract only the week part of it, and count the number of matches per week for the team.

The output is similar to our earlier validations. Now we will go ahead and correct the schedule for this team.

```
#Practically, a team can play a match per week and let us identify teams having more than one game per week
sport_schedule_NYG.loc[:,"week_nbr"]=sport_schedule_NYG['start_date_time'].dt.isocalendar().week
sport_schedule_NYG.groupby(['week_nbr'])['week_nbr'].count()
```

week_nbr
34 1
35 1
36 1
37 1
38 2
41 4
45 4
49 4
Name: week_nbr dtype: int64

Figure 9.60: Validating whether there is only one match per week for NFL teams

We will use the numpy library and its where clause function to identify records using their start dates and also the opposing team when there is duplicate data for the start date. The following update statements are only for learning purposes in this book. We have updated the schedule manually to ensure there is only one match per week for the New York Giants team, but we haven’t compared the schedule of other teams, and in reality, it would be a complex exercise to create a schedule reflecting this. So, we will only focus on the New York Giants and assume the schedule for other teams will not have any conflicts: