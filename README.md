**REQUIRED FILES** (inside RAR)

- Curl.exe

- PVupload\_Constants.txt

- PVupload\_Live.vbs

- PVupload\_Live.log

- PVupload\_Update.vbs

- PVupload\_Update.log

- PVupload\_Backdate.vbs

- PVupload\_Backdate.log

- PVupload\_TwoDays.vbs

- PVupload\_Return.log

- README.txt (this file)



**WWW.PVOUTPUT.ORG**

This script is designed for uploading SMA inverter data to pvoutput.org - hence an account needs to be created on pvoutput.org.

Create an account first and log yourself in.

Go to the Settings page and you can then Enable API Access.

Generate a new API key - which will then be added to the PVupload\_Constants.txt file.

Also take note of your system ID as this is required for the constants file.


**INSTALLATION**

- Open RAR file and extract all files to working directory for Sunny Explorer (SE) - generally C:\Users\<local account>\Documents\SMA\Sunny Explorer

- Double click on PVupload\_Constants.txt to edit in notepad.

- There are a number of constants that need to be set before the vbs scripts can be run (See CONSTANTS section below for examples). They are:

> - API\_KEY - assigned by pvoutput.org. Should be in quotes - ie API\_KEY="sasdfgasdfgasdfgasdfgh"

> - SYSTEM\_ID - assigned by pvoutput.org. Should be in quotes - ie SYSTEM\_ID="123"

> - SE\_Path - This is the install path of the Sunny Explorer APPLICATION. Will be somewhere under Program Files. Path should end with \

> - Data\_Path - This is the Path that Sunny Explorer uses to Save CSV files - and should be where the above files have been saved. Path should end with \

> - Exp\_Path - Same as Data\_path but without the trailing backslash

> - SE\_Name - This is the system name of your PV install in Sunny Explorer. There should be a .sx2 file with this name in the SE directory

> - SE\_Extension - This will be either sx2 or sxp (not sure why SE uses different extensions). Make sure there is no dot in front.

> - User\_Pwd - This is the password used to open your Plant in Sunny Explorer in USER mode. Default is 0000.

> - Update\_Interval - Set to 10min or 5min Update Intervals. Match with pvoutput via Settings -> Edit -> Status Interval.

- Save the changes to PVupload\_Constants.txt.

- Installation is now complete.


**IMPORTANT SETTING - CLOCK**

For the script to work correctly, SE must be exporting the CSV file with 24hr time format (HH:mm).

To check/update this setting do the following:

- Open SE and on the left hand side select Sunny Explorer (under your plant name).

- Click on the 'Settings' tab.

- Go to Device and then hit the Edit button at the bottom.

- Ensure the Time Format is set to HH:mm.

- Save any changes

- Close SE.


**CONSTANTS - Examples**

Layout of constants if running Windows 7/Vista:


API\_KEY="sasdfgasdfgasdfgasdfgh"


SYSTEM\_ID="123"


SE\_Path="C:\Program Files (x86)\SMA\Sunny Explorer\"


Data\_Path="C:\Users\Gary\Documents\SMA\Sunny Explorer\"


Exp\_Path="C:\Users\Gary\Documents\SMA\Sunny Explorer"


SE\_Name="TheGreatGazolio"


SE\_Extension="sx2"


User\_Pwd = "0000"


Update\_Interval = 5



Layout of Constants if using Windows XP:


API\_KEY="sasdfgasdfgasdfgasdfgh"


SYSTEM\_ID="123"


SE\_Path="C:\Program Files\SMA\Sunny Explorer\"


Data\_Path="C:\Documents and Settings\Gary\My Documents\SMA\Sunny Explorer\"


Exp\_Path="C:\Documents and Settings\Gary\My Documents\SMA\Sunny Explorer"


SE\_Name="TheGreatGazolio"


SE\_Extension="sx2"


User\_Pwd = "0000"


Update\_Interval = 5



**NO LONGER REQUIRED - MICROSOFT EXCEL**

As of Version PV4.0, Excel is no longer required. A big thanx to David Scott for working out the coding for this.

Older versions (PV3.1 and prior) use microsoft Excel to read the CSV data exported from Sunny Explorer and then upload the relevant details to pvoutput.


**LIVEORUPDATE OPTION**

The LiveOrUpdate option has been removed from version PV3.0.

Instead, the scripts are now separated in to Live, Update and Backdate mode.

Each script is described further below.


**LIVE MODE**

The LIVE script is designed for live logging of data to pvoutput and should be run every 10 minutes (see AUTOMATIC APPLICATION below for
instructions on how to use Windows Scheduler to automate this process).

It will update the CSV file and then upload the last 4 entries to pvoutput. So, if you are running 5min intervals, the last 15 minutes will be covered.

If you are running 10min intervals, the last 30 minutes will be covered. Existing entries will be updated.

It is recommended to set your system up to log status intervals of 5 minutes as it gives a cleaner graph.

To change the setting in pvoutput, log in and go to settings -> Edit (next to System Id) -> Status Interval and change to 5mins.

This option lines up with the Constant 'Update\_Interval' so make sure both are set to the same thing.

There is no plan to include the status interval of 15mins as an option.


**UPDATE MODE**

The UPDATE script is primarily designed to be run once at the end of the day to catch up all live data for the day.

Previous versions of this script took up to 2 hours to run but it is now using the 'AddBatchStatus' API option (allowing 30 entries at once) and

therefore only takes a couple of minutes to run.

It is recommended to schedule to script to run automatically via the Windows Scheduler - sometime after sun-down if your inverter allows connection.

If your Inverter does not allow connection after sun-down, I recommend running the new script PVupload\_TwoDays.vbs sometime during the day. See details below.

It is also possible to run the UPDATE script periodically throughout the day if you would prefer not to have your PC/laptop on all the time.

Each time it is run, all data for the day will be re-uploaded to pvoutput.

There is a variable just under the CONSTANTS section called 'ReloadCSVfromInverter'. When set to 1, the script will attempt to connect to the

inverter and download the CSV file for that day's data. If you do NOT want to do this (ie you have been running the LIVE script all day and now

want to run the UPDATE script to fill some holes in the data) then switch the variable to 0 and it will skip the step of logging on to the inverter.


**BACKDATE MODE**

The BACKDATE script has now been added to allow you to catch up live data up to 14 days in the past (or 90 days if you have donated to pvoutput).

This script can only be run manually.

When the script is run, you will be prompted with a date field (automatically pre-filled with the current date). Update this date - using the

format YYYYMMDD where YYYY=year, MM=Month and DD=Day - to the date you want to upload data for.

There is a variable just under the CONSTANTS section called 'ReloadCSVfromInverter'. Set to 1, the script will attempt to connect to the

inverter and download the CSV files for that day's data. If you do NOT want to do this (ie you have all the CSV files from previously running

the Live or Update scripts) then switch the variable to 0 and it will skip the step of logging on to the inverter.


**TWODAYS SCRIPT**

This script has been added for users that only want to log on once a day and have all live data up-to-date.

It can also be handy for users that have piggyback bluetooth cards which do not allow connection to the inverter when it is 'sleeping' (ie once the sun goes down).

The script can be run manually (by double clicking on the vbs file) or by using Windows Scheduler.

It will log on to the inverter and download today's data along with all of yesterday's data.

Today's data will then be uploaded to pvoutput.org.

Yesterday's data will then be uploaded to pvoutput.org, overwriting any data written previously.

The idea is that if you run it once a day during the day (say midday), it will aways ensure that the previous day's data is up to date, as well as

loading all of today's data.

There is no separate log file for the TwoDays script - it uses the BackDate log file.



**AUTOMATIC APPLICATION**

Windows scheduler (Task Scheduler) can be used to schedule the Live or Update script.

For the LIVE script, it is recommended to run the script every 10 minutes.

In Task Scheduler:

> - Actions. Start a program. Browse to PVupload.vbs and select

> - Triggers. Run daily starting 06:01AM to recur every 1 days. Repeat task every 10minutes for 12-15 hours.

> - Seems to be a good idea to force a timeout for the script as well. Under 'settings' (Windows7), Activate 'Stop the task if it runs

> longer than:' and set to 7 minutes.

> - Tweek as required.

For the UPDATE script, it is designed to be run once at the end of the day but can also be run periodically (eg every hour) to catch up data.

In Task Scheduler:

> - Actions. Start a program. Browse to PVupload.vbs and select

> - Triggers. Run daily starting 21:01PM to recur every 1 days.

> OR run daily starting 07:01AM to recur every 1 days. Repeat task every 1 hour for 12-15 hours.

> - Seems to be a good idea to force a timeout for the script as well. Under 'settings' (Windows7), Activate 'Stop the task if it runs

> longer than:' and set to 7 minutes.

> - Tweek as required.


NOTE: Sunny Explorer can not remain open while the script runs as it requires the SE service to extract data.

Any open session of SE will be closed immediately when the scrip runs.



**MANUAL APPLICATION**

The PVupload.vbs script can be manually run at any time by double clicking in windows.

Bluetooth can take up to 2 minutes to connect to SE and extract data.

Once complete, the date/time stamp on the current SE CSV file will be updated.

Shortly after, the date/time stamp on the PVupload.log file will be updated and you can open this file to see what was entered.

If data was uploaded, pvoutput.org should be updated within 1 minute


**DONATIONS**

Donations: I have been asked a few times about accepting donations for this project. Please prioritize donations to the pvoutput.org website but if you wish to contribute to the PVupload project then you are welcome to send a donation via paypal to gazglids@picknowl.com.au. Thank you.

**CHANGE LOG**

V4.4
- Thanx to new version of Sunny Explorer (V1.07.09 onwards) the export format of the CSV data file is now Unicode (previously ANSII).
Scripts have been updated to handle Unicode rather than ASCII. SMA have not been able to confirm if this change is 'by design'.


V4.3

- Removed 'Number Format' checking from V4.2. Now stripping all commas and dots out of the WattHour and Watt readings so PC based Number Format
settings are no longer relevant. Makes install and setup easier for international users.

- Vastly improved error logging. All errors read the return code from PVoutput and write it to the log file so diagnosing issues is much easier.

- The Backdate script can run up to 14 days of live data in the past or up to 90 days if you have donated to pvoutput.
This is not limited in the script. If the pvoutput developer changes it again, the script will adapt.
When it hits the limits it uses the return code from pvoutput and writes it to the log.

- All the update scripts (Update, Backdate and Twodays) now write 30 data items per batch to pvoutput (doubled from 15) and each batch takes only 1 API call (previously 4 per batch). With 60 API calls available per hour (or 300 API calls if you have donated to pvoutput) you can run the scripts many times an hour (great for catching up on missed data).

V4.2

- Added error checking for 'Number Format' in Sunny Explorer. If set to 'comma', an error will be written to log file. Needs to be set to 'dot' (ie 123,456.0)

- Added error checking to multi-inverter setup for when only one inverter kicks in at a time.

V4.1

- Corrected coded to match new functionality in Sunny Explorer V1.04.23. The CSV now exports with seconds (HH:mm:ss) so coding had to be updated to match this change

- Added support for 4th Inverter so if data is extracted to CSV for up to 4 inverters then all data will be uploaded to pvoutput.org.

V4.0

- Added new script - PVupload\_TwoDays.vbs. See details above for its function.

- Removed all references to Excel so that the scripts can be run without Office being installed. The scripts now use inbuilt windows scripting functions.

V3.1

- Removed constants from individual scripts and placed in separate file - PVupload\_Constants.txt. This makes future upgrades easier and saves having to update constants in each of the scripts.

- Fixed bug in PVupload\_Backdate.vbs script where checking of dates was not working correctly. Now using datediff function to compare dates.

- Added pause and wait functions in to UPDATE and BACKDATE scripts so that when the '60 requests per hour' limit is reached on pvoutput, rather than bombing out, the script pauses for 10mins and retries - until the request limit is lifted and the data completes loading.

- Added new file PVupload\_Return.log to capture return commands from CURL uploads. This allows improved error checking and control of the pause functionality in the UPDATE and BACKDATE scripts.

V3.0

- PVupload.vbs now split in to 2 scripts -> PVupload\_Live.vbs and PVupload\_Update.vbs.

- Separate Log files created for each script (which allows the scripts to be re simultaneously) -> PVupload\_Live.log and PVupload\_Update.log.

- Live script now only loads 4 entries each run (previously 6). This allows some wiggle room in the '60 requests per hour' limit set by pvoutput.

- Update script now loads an entire day of data in under 2 minutes (previously took up to 2 hours).

- Removed LiveOrUpdate Constant.

- Added Debugging variable which can be switched on (on=1,off=0) to write more details to log file (including status responses from pvoutput).

- Added 3rd script - PVupload\_Backdate.vbs - which allows live data to be loaded up to 14 days prior (One day at a time).

- Added log file for Backdate script -> PVupload\_Backdate.log.

- Rebuild of this README including addition of CONSTANTS - Examples.


V2.1

- Added new constant - User\_Pwd. This is the password used to log in to the USER account for your plant on Sunny Explorer.
By default this password is 0000 but some people like to change it - so if you have changed it in SE then you can update the script to reflect that.

- Altered process for running in UPDATE mode. Prompt now appears at the start of the script to ensure you actually want to

be running the UPDATE option and gives you the option to cancel out. The UPDATE mode has also been slowed right down so that it only adds 1 entry every minute to comply with pvoutput requirements - which means it can take up to 2 hours to run!


V2.0

- totally rebuilt main script so that it now reads the entire CSV file every time it is run. All data is now stored in arrays and
can be retrieved each run.

- Added ability to write data to pvoutput in 5min blocks instead of 10min blocks. Gives a cleaner graph on pvoutput. To use
this option, need to update constant 'Update\_Interval in PVupload.vbs script as well as changing setting in pvoutput.
To change the setting in pvoutput, log in and go to settings -> Edit (next to System Id) -> Status Interval.
NOTE: There is no need to run the script every 5 mins. Continue to run it at 10min intervals and all information will be added automatically.

- when run in 'live' mode (via constant setting), each time the script is run it will upload the current yield/power to pvoutput along
with the previous 5 entries.
This helps to automatically fill holes in your pvoutput data where bluetooth has failed on the previous attempt or if
PC was not running when the script would normally have run.

- when run in 'update' mode (via contstant setting), the script should only be run ONCE at the end of the day. The script will
load/reload the entire day's data into the pvoutput live screen allowing you to see the full day of live data without having your PC on all day.

- Improved information being written to log file.

- added error checking for starting point on all 3 inverters (rather than just 2 and 3) incase inverter 2 kicks in first.


V1.7

- Added catchup entries. This will load the two previous time stamps as well as the current time stamp to pvoutput.
This helps to automatically fill holes in your pvoutput data where bluetooth has failed on the previous attempt or if
PC was not running when the script would normally have run.

- Altered detail written to log file to show both current time and the time stamp being written to pvoutput.

- Modifications to error checking when bluetooth fails to connect.

V1.6

- Fixed minor typo in main script that stopped the date being written to the log file.

V1.5

- Improved support for multiple inverters (picks up on a number of quirks with the SE data extraction process).

- Significant changes made to both upload script and catch-up script around pick-up of yield and watt amounts.


V1.4

- Added support for up to 3 inverters per SE Power Plant (can be extended if required). Thanx to Big Dam (Roger) for testing on multiple inverters.

- Added second script to manually add up to 10 data entries to pvoutput at one time. See above for how this is used.


V1.3

- Added Constant SE\_Extension to allow input of extension. SE uses either sx2 or sxp for file extension.