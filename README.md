# Branch Query API Google Sheets Scripts

## What do they do?

The query scripts will pull commerce data each day for Ad Networks and populate the Google Sheet accordingly. Currently, you can only do one aggregation at a time.

## Installation:

1. Add a sheet to your Google Sheet and name it Settings
2. Add 4 rows to the sheet with the names:
	- Branch Key
	- Branch Secret
	- Query Limit
	- Ad Networks
	- Aggregation
3. For each of those rows, populate with the following:
	- Branch Key: Retrieve from the Account Settings section of the Branch Dashboard
	- Branch Secret: Retrieve from the Account Settings section of the Branch Dashboard
	- Query Limit: Set this by default to 1000 - this is capped at 10 000 rows.
	- Ad Networks: Populate with a comma separated list of ad network names. You can retrieve this list from the list from the Branch dashboard under Ads -> Partner Management.
		Note: the names are case sensitive and need to be input exactly as they appear on the Branch dashboard. e.g. Google AdWords *not* Google adwords
	- Aggregation: Possible values - `revenue`, `total_count`, `unique_count`, `cost` for description of these values, see here: https://help.branch.io/developers-hub/docs/query-api#section-data-selection

![Settings](docs/Settings.png)

4. Once Settings are complete in the sheet, select Tools -> Script Editor from the menu
5. The script editor will open. Give the project a name e.g. Branch Query Scripts
6. Replace the function 
```
myFunction() {
	
}
```

with

```
function dailyExecution(evt) {
  Logger.log(`dailyExecution: ${JSON.stringify(evt)}`)
  const yesterday = Query.removeDay(new Date())
  Query.updateSheet(yesterday)
}
```

![Code](docs/Code.png)

7. Save the script
8. Select Resources -> Libraries from the menu
9. In the Add Library section copy and paste this Script ID: 1j5qtnmGRka-wUt96WXEBeWUq_q8r7wvk2UZ4IWttQZPIdl0isKUuGQnk and press Add

![LinkLibrary](docs/LinkLibrary.png)

10. Replace the Identifier with `Query` and make sure the latest Version is selected

![LibrarySettings](docs/LibrarySettings.png)

11. Press Save
12. From the menu Select Edit -> Current Project's Triggers
13. In the new tab that opens: Add a Trigger
14. Set the Event Source to Time-driven and the Timer to Day Timer (the defaults for time are fine) and press Save

![Trigger](docs/Trigger.png)

15. You will need to give permission to the scripts to access your Google account. This is required in order to edit the sheet. Note: If your browser blocks popups, you might need to allow popups in order to run through verification.

![AuthTrigger](docs/AuthTrigger.png)
![AuthTrigger2](docs/AuthTrigger2.png)

That's it. When the trigger is executed you will see the data appear as separate sheets for each Ad Network. Just add more Ad Networks (Step 3) if you want their data to start showing up.

