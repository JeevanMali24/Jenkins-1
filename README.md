# Jenkins-1


## Installation on EC2 Instance

YouTube Video ->
https://www.youtube.com/watch?v=zZfhAXfBvVA&list=RDCMUCnnQ3ybuyFdzvgv2Ky5jnAA&index=1


![Screenshot 2023-02-01 at 5 46 14 PM](https://user-images.githubusercontent.com/43399466/216040281-6c8b89c3-8c22-4620-ad1c-8edd78eb31ae.png)

Install Jenkins, configure Docker as agent, set up cicd, deploy applications to k8s and much more.

## AWS EC2 Instance

- Go to AWS Console
- Instances(running)
- Launch instances

<img width="994" alt="Screenshot 2023-02-01 at 12 37 45 PM" src="https://user-images.githubusercontent.com/43399466/215974891-196abfe9-ace0-407b-abd2-adcffe218e3f.png">

### Install Jenkins.

Pre-Requisites:
 - Java (JDK)

### Run the below commands to install Java and Jenkins

Install Java

```
sudo apt update
sudo apt install openjdk-17-jre
```

Verify Java is Installed

```
java -version
```

Now, you can proceed with installing Jenkins

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

- EC2 > Instances > Click on <Instance-ID>
- In the bottom tabs -> Click on Security
- Security groups
- Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed `All traffic`).

<img width="1187" alt="Screenshot 2023-02-01 at 12 42 01 PM" src="https://user-images.githubusercontent.com/43399466/215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22.png">


### Login to Jenkins using the below URL:

http://<ec2-instance-public-ip-address>:8080    [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]

Note: If you are not interested in allowing `All Traffic` to your EC2 instance
      1. Delete the inbound traffic rule for your instance
      2. Edit the inbound traffic rule to only allow custom TCP port `8080`
  
After you login to Jenkins, 
      - Run the command to copy the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
      - Enter the Administrator password
      
<img width="1291" alt="Screenshot 2023-02-01 at 10 56 25 AM" src="https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png">

### Click on Install suggested plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 58 40 AM" src="https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png">

Wait for the Jenkins to Install suggested plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 59 31 AM" src="https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png">

Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]

<img width="990" alt="Screenshot 2023-02-01 at 11 02 09 AM" src="https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png">

Jenkins Installation is Successful. You can now starting using the Jenkins 

<img width="990" alt="Screenshot 2023-02-01 at 11 14 13 AM" src="https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png">

## Install the Docker Pipeline plugin in Jenkins:

   - Log in to Jenkins.
   - Go to Manage Jenkins > Manage Plugins.
   - In the Available tab, search for "Docker Pipeline".
   - Select the plugin and click the Install button.
   - Restart Jenkins after the plugin is installed.
   
<img width="1392" alt="Screenshot 2023-02-01 at 12 17 02 PM" src="https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png">

Wait for the Jenkins to be restarted.


## Docker Slave Configuration

Run the below command to Install Docker

```
sudo apt update
sudo apt install docker.io
```
 
### Grant Jenkins user and Ubuntu user permission to docker deamon.

```
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

Once you are done with the above steps, it is better to restart Jenkins.

```
http://<ec2-instance-public-ip>:8080/restart
```

The docker agent configuration is now successful.





# To schedule a project in Jenkins, you can use Jenkins' built-in "Build Triggers" feature. Here's how you can do it:

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

Scheduling a project in Jenkins means setting up automated builds that run at specific times or intervals without manual intervention. Jenkins offers a feature called **Build Triggers**, which allows you to configure when and how often a job (project) should run. One of the common triggers is **Build periodically**, which uses a cron-like syntax to define the schedule.

### Step-by-Step Explanation:

1. **Access Jenkins Dashboard**:
   - After logging into Jenkins, you will see the list of projects (also called "jobs"). You select the one you want to automate.

2. **Open Project Configuration**:
   - In Jenkins, each project has a configuration page where you define how it behaves. You can access this by clicking on the project and selecting the `Configure` option from the left-hand menu.

3. **Find the Build Triggers Section**:
   - Jenkins offers different options to trigger builds, such as manual builds, triggered by changes in source code (like Git), or time-based triggers (which is what you are setting up). The **Build Triggers** section is where you enable these options.

4. **Enable Periodic Scheduling**:
   - To schedule a job to run at specific times, check the **Build periodically** option. This tells Jenkins that you want the project to run automatically according to a defined time schedule.

5. **Use Cron Syntax**:
   - Jenkins uses a scheduling format similar to **cron**, a time-based job scheduler in Unix-like systems.
   - This format allows you to specify:
     - **Minute** (0-59)
     - **Hour** (0-23)
     - **Day of Month** (1-31)
     - **Month** (1-12)
     - **Day of Week** (0-7, where both 0 and 7 represent Sunday)
   - Example:
     - `0 2 * * *`: This will run the job every day at 2 AM.
     - `H/15 * * * *`: This will run the job every 15 minutes (the `H` helps distribute the load across different times).

   The flexibility of cron syntax allows you to set very specific schedules, such as running a job every Monday at 8 AM or the first day of every month.

6. **Save the Configuration**:
   - After entering the cron expression that defines your schedule, you save the changes. This means Jenkins will now automatically trigger the project at the defined times.

7. **Build History**:
   - After setting up the schedule, Jenkins will run the job based on the time intervals you specified, and you can see the results in the **Build History** section. This shows when the job ran, its status (success or failure), and links to logs.

### Why Use Scheduling in Jenkins?
- **Automation**: Saves time and effort by automatically running tests, builds, or deployments without needing manual intervention.
- **Consistency**: Ensures the same job is executed at regular intervals (e.g., every night or every week) to maintain consistency.
- **Continuous Integration**: Fits well with CI/CD pipelines, where you want frequent, automated builds that check for errors or new changes in code.

Scheduling projects is particularly useful for tasks like:
- Nightly builds.
- Running automated tests at regular intervals.
- Periodic code quality checks.

Let me know if you need more details on any part of this!

# Let me explain the concept of **upstream** and **downstream** projects in Jenkins with a visual aid:

### Concept Overview:
- **Upstream Project**: A job that runs first and may trigger another job upon completion.
- **Downstream Project**: A job that is triggered by the upstream project once it completes.

### Workflow Example:
Imagine you have three Jenkins jobs:
1. **Project A** (Build) - The project that compiles your code.
2. **Project B** (Test) - After the build is successful, this project runs tests.
3. **Project C** (Deploy) - Once tests pass, this project deploys the code.

Here’s the upstream-downstream relationship:
- **Project A** (Build) is the **upstream** project for **Project B** (Test).
- **Project B** (Test) is the **downstream** project of **Project A** but the **upstream** project for **Project C** (Deploy).
- **Project C** (Deploy) is the **downstream** project for both.

### Diagram

Let me create a simple diagram for this.

![DALL·E 2024-10-06 11 54 46 - A simple diagram illustrating Jenkins upstream and downstream projects workflow  The diagram has three blocks aligned vertically with arrows between t (1)](https://github.com/user-attachments/assets/7341e8d8-0031-4130-aefd-54ebc1ad0df1)


Here is a simple diagram illustrating the upstream and downstream project workflow in Jenkins. It shows how **Project A (Build)** triggers **Project B (Test)**, and once that completes, it triggers **Project C (Deploy)**. This visual represents the flow of jobs from building to testing and then deploying.
