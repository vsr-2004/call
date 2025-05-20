FROM openjdk
WORKDIR /app
COPY . /app
RUN javac Add.java
CMD ["java","Add"]

docker build -t img1 .
docker run img1

docker login
docker images
docker tag img1:latest samarthh23/add-app
docker push samarthh23/add-app
docker pull samarthh23/add-app

FROM python:3.12.9-slim
WORKDIR /app
COPY  app.py .
CMD ["python","app"]


END 5



6 th



1️⃣ Create Maven Project

In Eclipse:

File → New → Other...

Search for Maven Project → Click Next

Choose Create a simple project → Next

archetype-quickstart

Group Id: com.samarth.jsonreader

Artifact Id: jsonproject

Name: jsonproject

Finish


✅ Eclipse creates:

jsonproject/
├── src/
│   ├── main/
│   │   └── java/
│   └── test/
│       └── java/
├── pom.xml


---

2️⃣ Add a JSON file

Right-click the project → New → Folder → Name it JSON

Inside the JSON folder, create a file like data.json


Example data.json:

{
  "firstname": "Samarth",
  "lastname": "Hegde"
}



---

3️⃣ Add JSON dependency in pom.xml

1. Go to https://mvnrepository.com


2. Search for json-simple


3. Choose a version (like 1.1.1)



Add this to pom.xml inside <dependencies>:


<!-- https://mvnrepository.com/artifact/com.googlecode.json-simple/json-simple -->
<dependency>
    <groupId>com.googlecode.json-simple</groupId>
    <artifactId>json-simple</artifactId>
    <version>1.1.1</version>
</dependency>

Then right-click your project → Maven → Update Project


---

4️⃣ Create Java class to read JSON

In src/test/java, create a class:
Right-click → New → Class → Name: JsonReader

Paste the following code:

package maven.lab6;

import java.io.FileReader;
import java.io.IOException;

import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;

public class demo {
    public static void main(String[] args) throws IOException, ParseException {
        JSONParser jsonparser = new JSONParser();
        FileReader reader = new FileReader(".\\JSON\\data.json");  
        Object obj = jsonparser.parse(reader);
        JSONObject empjsonobj = (JSONObject) obj;

        String fname = (String) empjsonobj.get("firstname");
        String lname = (String) empjsonobj.get("lastname");

        System.out.println("First name: " + fname);
        System.out.println("Last name: " + lname);
    }
}



---

5️⃣ Run and Test

Right-click on JsonReader.java → Run As → Java Application
✅ You should see output:

Name: Samarth
Age: 22
Branch: ISE


---

🎯 Final Output

You now have a Maven project that:

Uses a dependency (from Maven Central)

Reads from a .json file

Can be pushed to GitHub and used in Jenkins for CI/CD



---

Do you want me to explain how to integrate this with Jenkins or Docker in your next DevOps step?





END 6







7th


🧩 PART 1: Slack Setup

1️⃣ Install Slack & Create Workspace

Go to https://slack.com

Create an account or log in

Create a workspace (e.g., devops-lab)

Inside the workspace, create a channel (e.g., #build-status)



---

🧩 PART 2: Slack Integration in Jenkins

2️⃣ Install Slack Notification Plugin

Open Jenkins → Manage Jenkins → Manage Plugins

Under Available, search for: Slack Notification Plugin

Install it and restart Jenkins if needed



---

3️⃣ Generate Slack Integration Token

In Slack, go to this link: https://my.slack.com/services/new/jenkins-ci

Select your workspace and the channel (e.g., #build-status)

Click Add Jenkins CI integration

It will generate a "Integration Token Credential ID"
Example: abcdefgh12345678



---

4️⃣ Configure Slack in Jenkins

Jenkins → Manage Jenkins → Configure System

Scroll to Slack section:

Workspace: yourworkspace.slack.com

Credential ID: Paste the token

Channel: #build-status

Click Test Connection to verify




---

5️⃣ Create Jenkins Job with Slack Notification

New Item → choose Freestyle project → name it slack-notify

Go to Post-build Actions:

Choose Slack Notifications

Select when to notify: Success, Failure, etc.



Save and Build the project
✅ You will receive a message in Slack like:
Build #1 of slack-notify: SUCCESS


---

🧩 PART 3: Email Notifications

6️⃣ Configure Email in Jenkins

Jenkins → Manage Jenkins → Configure System

Scroll to E-mail Notification

SMTP Server: (e.g., smtp.gmail.com)

Click Advanced:

Use SMTP Authentication

User: your Gmail ID

Password: App password (not your Gmail password)

SMTP Port: 465 (SSL) or 587 (TLS)

Reply-To Address: your email

Check Use SSL




Click Test configuration by sending test e-mail.


---

7️⃣ Add Email Post-build Action

In your Freestyle project:

Go to Post-build Actions → Editable Email Notification

Add recipient email (e.g., you@gmail.com)

Add trigger for: Failure, Success, or Always

Save and build again


✅ You will get an email with build status


---

✅ Summary

Task	Tool	Where It Happens

Slack Notification	Slack + Jenkins Plugin	Jenkins → Slack
Email Notification	SMTP Config in Jenkins	Jenkins → Email
Post-Build Configuration	Freestyle Project	Jenkins Job



---

Do you want a sample screenshot or help testing it locally?
