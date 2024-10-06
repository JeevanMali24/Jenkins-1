# Jenkins-1

To schedule a project in Jenkins, you can use Jenkins' built-in "Build Triggers" feature. Here's how you can do it:

### Steps to Schedule a Project in Jenkins:
1. **Access Jenkins Dashboard**:
   - Open Jenkins and log in with your credentials.

2. **Open the Project**:
   - Select the project (job) you want to schedule from the Jenkins dashboard.

3. **Configure the Project**:
   - Click on the `Configure` option in the left-hand menu of the project.

4. **Scroll Down to Build Triggers**:
   - Look for the **Build Triggers** section in the project configuration.

5. **Enable Build Trigger by Schedule**:
   - Check the box for **Build periodically**. This option allows you to schedule the build using a cron-like syntax.

6. **Set the Schedule Using Cron Syntax**:
   - You can now input your schedule using Jenkins' cron format. The syntax is:
     ```
     MINUTE HOUR DOM MONTH DOW
     ```
     - **MINUTE**: Minute (0–59)
     - **HOUR**: Hour (0–23)
     - **DOM**: Day of the month (1–31)
     - **MONTH**: Month (1–12)
     - **DOW**: Day of the week (0–7) (Sunday is both 0 and 7)
   
   Example schedules:
   - Every 15 minutes: `H/15 * * * *`
   - Every day at midnight: `0 0 * * *`
   - Every Monday at 8 AM: `0 8 * * 1`

   The `H` character can be used to distribute builds evenly across a time range.

7. **Save the Configuration**:
   - Once you've entered the schedule, scroll down and click the `Save` button.

8. **Check the Build History**:
   - The job will now run according to the schedule you've defined, and you can check its history and output under the **Build History** section.

### Example:
To schedule a project to run every day at 3 AM, use this cron syntax:
```
0 3 * * *
```

This will automatically trigger the build at 3 AM daily.

Let me know if you need any additional details!
