1. Set Up GitHub Repository

Step 1:Create a GitHub Repository:
![image](https://github.com/user-attachments/assets/9a6878f9-6adb-4085-beba-1dd7bcff159c)


Step 2:Go to GitHub and create a new repository to store your data file (data.json).

Step 3:Create Personal Access Token:

Go to Settings > Developer settings > Personal access tokens.
Generate a new token with appropriate scopes (e.g., repo for private repos or public_repo for public repos).

2. Automate Google Sheets to GitHub

Step 1:Script to Update GitHub

Step 2:Open Google Sheets:

Step 3:Go to Extensions > Apps Script to open the Apps Script editor.

Step 4:Write the Script ----> function updateGitHub() {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = sheet.getDataRange().getValues();
  
  var jsonData = JSON.stringify(data);
  var githubUrl = 'https://api.github.com/repos/OWNER/REPO/contents/data.json'; // Replace OWNER and REPO
  var token = 'YOUR_GITHUB_TOKEN'; // Replace with your token
  var sha = getFileSHA(); // Fetch the latest SHA before updating
  
  var options = {
    method: 'put',
    headers: {
      Authorization: 'token ' + token,
      'Content-Type': 'application/json'
    },
    payload: JSON.stringify({
      message: 'Update from Google Sheets',
      content: Utilities.base64Encode(jsonData),
      sha: sha
    })
  };
  
  var response = UrlFetchApp.fetch(githubUrl, options);
  Logger.log(response.getContentText());
}

function getFileSHA() {
  var token = 'YOUR_GITHUB_TOKEN'; // Replace with your token
  var githubUrl = 'https://api.github.com/repos/OWNER/REPO/contents/data.json'; // Replace OWNER and REPO
  
  var options = {
    method: 'get',
    headers: {
      Authorization: 'token ' + token,
      'Content-Type': 'application/json'
    }
  };
  
  var response = UrlFetchApp.fetch(githubUrl, options);
  var fileData = JSON.parse(response.getContentText());
  Logger.log('Current SHA: ' + fileData.sha);
  return fileData.sha;
}

Step 5:Save and Authorize: Save the script and authorize it to access Google Sheets and make external requests.

After Above 5 steps is done:

2）Set up Trigger: 

Step 1:Open Triggers Page

Step 2:In the Apps Script editor, click on the clock icon or go to Edit > Current project's triggers. 

Step 3:Add a Trigger: Click + Add Trigger.

Step 4:Set the function to updateGitHub.

Step 5:Choose the event source as Time-driven if you want periodic updates.

Step 6:Set the time interval (e.g., daily, hourly).

3. Manage Versions and Backups
   
1) Backup Google Sheets Regularly:
Create a backup of your Google Sheets data to another sheet or a different format on a regular basis. This can be done manually or via a separate script if desired.

2)Use GitHub Version History:
Utilize GitHub’s version history to manage and restore older versions of the file if needed.

3)Manually Restore Files:
If necessary, manually restore files from GitHub’s commit history to revert to previous versions.

4)Summary
Update Google Sheets to GitHub: Set up a script to push data from Google Sheets to GitHub and configure a time-driven trigger for automation.
Manage Versions: Utilize GitHub’s commit history and version control to handle file versions.

This setup ensures that data is regularly updated from Google Sheets to GitHub, while giving you control over managing file versions and backups.






