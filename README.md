![image](https://github.com/user-attachments/assets/067ad049-cf2d-455b-97d3-6a0f3edba141)1. Set Up GitHub Repository

Step 1:Create a GitHub Repository:
![屏幕截图 2024-07-31 224514](https://github.com/user-attachments/assets/96405418-d7b4-4b33-aace-6df9a8c54ac7)

Step 2:Go to GitHub and create a new repository to store your data file (data.json).
![屏幕截图 2024-07-31 224658](https://github.com/user-attachments/assets/88fcbb28-f7ea-4391-a9f6-d8c2e97c0611)

Step 3:Create Personal Access Token:

Go to Settings > Developer settings > Personal access tokens.
![屏幕截图 2024-07-31 224754](https://github.com/user-attachments/assets/1276cf10-6572-4899-b630-1854095df741)
![屏幕截图 2024-07-31 224942](https://github.com/user-attachments/assets/110e2a49-ae94-4288-be08-83792414cc41)
![屏幕截图 2024-07-31 225129](https://github.com/user-attachments/assets/11a07b86-fcf7-488e-a35a-186bda049a36)
![屏幕截图 2024-07-31 225211](https://github.com/user-attachments/assets/af4e5ba4-eeed-4b6f-ab26-180aa016dd01)
![屏幕截图 2024-07-31 225253](https://github.com/user-attachments/assets/ff96a21d-18f6-412e-932d-630534bdf0ac)
![屏幕截图 2024-07-31 225347](https://github.com/user-attachments/assets/696e61ce-a416-46a7-b9ed-480b66bf58d2)
![屏幕截图 2024-07-31 225419](https://github.com/user-attachments/assets/60f36d6f-96f0-42b6-8450-7882d190ec2d)

Generate a new token with appropriate scopes (e.g., repo for private repos or public_repo for public repos).

2. Automate Google Sheets to GitHub

Step 1:Script to Update GitHub


Step 2:Open Google Sheets:

Step 3:Go to Extensions > Apps Script to open the Apps Script editor.
![屏幕截图 2024-07-31 225517](https://github.com/user-attachments/assets/4d96a666-f72e-48e6-91c0-0648dfc3d7f0)

Step 4:
![image](https://github.com/user-attachments/assets/9dd969b9-295f-4e0c-9f74-4f5805190fe6)

Write the Script ---->
function updateGitHub() {
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
![屏幕截图 2024-07-31 225808](https://github.com/user-attachments/assets/80b2db7f-4255-4307-9a77-0234ee907c9d)


After Above 5 steps is done:

2）Set up Trigger: 

Step 1:Open Triggers Page
![屏幕截图 2024-07-31 230052](https://github.com/user-attachments/assets/8aa833fa-439d-4823-86a9-8edeca902450)

Step 2:In the Apps Script editor, click on the clock icon or go to Edit > Current project's triggers. 

Step 3:Add a Trigger: Click + Add Trigger.
![屏幕截图 2024-07-31 230142](https://github.com/user-attachments/assets/ae28b3ef-1fc6-427a-b1ad-e3f750f637cc)

Step 4:Set the function to updateGitHub.
![屏幕截图 2024-07-31 230353](https://github.com/user-attachments/assets/308292ec-a8b7-4549-97b7-828c3a72b900)

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






