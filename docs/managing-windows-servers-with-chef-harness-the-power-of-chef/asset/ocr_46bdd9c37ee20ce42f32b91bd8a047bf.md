| Parameter | Description |
|---|---|
| name | This is the resource name attribute specifying the name of the task (arbitrary name). |
| command | This is the command to execute on an interval. |
| cwd | This specifies the directory to run the command from. |
| user | This is the user to execute the command as. |
| password | This is the user's password. |
| run_level | The level of privileges to run with are listed as follows:

* :highest
* :limited

The default value is :limited. |
| frequency | This is the unit of time for which this command executes; the options are as follows:

* :minute
* :hourly
* :daily
* :weekly
* :monthly
* :once
* :on_login
* :onstart
* :on_idle

The default value is :hourly |
| frequency_modifier | This is the frequency interval such as 15, 2, 30, and so on. This is multiplied by the unit of time, so a modifier of 15 combined with :minute as the frequency would cause the command to repeat every 15 minutes.

The default value is 1. |
| start_day | This is an optional date to start the interval, formatted as MM/DD/YYYY. This is the first date on which the command is first executed and then runs on the specified frequency interval. The command runs immediately if this parameter is not specified. |
| start_time | This determines the time of the day to perform the first execution (immediately if nil). The format is HH:MM. |