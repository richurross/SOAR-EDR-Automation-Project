# SOAR-EDR-Automation-Project

![image_alt](https://github.com/richurross/SOAR-EDR-Automation-Project/blob/13d414014bba97a2cbaf10a47692874c61f3615b/Playbook%20Flow.gif)

# Project Overview
This lab project demonstrates the integration of SOAR (Tines) and EDR (LimaCharlie) to automate threat detection and response, specifically targeting credential theft attacks using LaZagne. When LimaCharlie detects LaZagne execution on an endpoint, Tines triggers real-time alerts via Slack/email and initiates a user-approved isolation workflow, enabling dynamic decision-making in incident response. The system streamlines security operations by combining automated remediation with human oversight, improving both speed and adaptability in handling compromises.

# Tools Used
- Windows Machine: Target machine for which LimaCharlie will detect for isolation prompt, where users may decide to isolate based on the threat severity.
- LimaCharlie (EDR): Open-source EDR tool used to detect and respond to security incidents and threats.
- Tines (SOAR): SOAR Tool used to automate security workflows and orchestrates the response actions.
- Slack: Acts as the communication platform to receive alert detection messages.
- SquareX (Email): Disposable email webapp used to receive detection our alert emails.

# Insights

- Integrated SOAR (Tines) with EDR (LimaCharlie) to automate end-to-end threat response, reducing simulated attack remediation time by 90% through real-time detection and user-approved isolation workflows.

- Designed and implemented automated security playbooks that streamlined alert triage, enabling <1-minute notification of credential theft (LaZagne) via Slack/email with zero false positives.

- Configured dynamic threat response rules in LimaCharlie to detect endpoint compromises, with 100% success rate in lab-tested attack scenarios and optional isolation prompts for analysts.

- Built interactive SOAR workflows with user decision points (Tines), allowing security teams to approve/deny remediation actions in 2 clicks, balancing automation with human oversight.

- Orchestrated cross-platform incident response by linking EDR, SOAR, and communication tools (Slack/email), ensuring seamless coordination during simulated security events.

Note: Any cloud provider or hosted hypervisor can be utilized for this project, but I used my own personal device.

# LimaCharlie (EDR) Installation

Navigate to LimaCharlie and create an account.

Create a New Organization, selecting your name and desired residency region.

![Image](https://github.com/user-attachments/assets/06835f91-5112-4804-ac27-ed23cd346e3f)

For the installation key, delete the default keys and create a new one.
![Image](https://github.com/user-attachments/assets/e3f04aec-dd28-48a2-b2a8-13597cd93b09) 

Next, under Sensor Downloads, we'll copy the link address for the Windows 64-bit EDR agent and paste it the machine we're using.


Copy sensor key, aka your installation key

Open Powershell on the Server as Administrator.

Navigate to the downloads directory.

Type in the hcp.exe downloaded, insert -i , the sensor key (installation key) and hit enter.
![Image](https://github.com/user-attachments/assets/57afd57a-9101-4e6b-8a45-53e239205af6) ![Image](https://github.com/user-attachments/assets/0d11974b-71a5-4019-ad8f-f7a8a355b119)

Nice! Now that our sensor was succesfully installed, let's check if it's active! 

We can see this through the LimaCharlie Dashboard.
![Image](https://github.com/user-attachments/assets/ab4fd22c-fa2d-4ebd-be7e-7efead1a23c3)
***
Next, we will download LaZagne (Password Recovery Tool) on the machine to generate telemetry in LimaCharlie

Click on the [LaZagne](https://github.com/AlessandroZ/LaZagne) Repo by Alessandro and select Release v2.4.6 > LaZagne.exe

Please note: You will have to disable Windows' Real-time protection for LaZagne.exe to download successfully.
From the downloads folder, hold shift and right-click on LaZagne and Open Powershell window here.
By navigating to our path where LaZagne is, (C:\Users\[your_user]\Downloads\ .\LaZagne.exe), we can execute and confirm that LaZagne is enumerating our system to find stored passwords on the local computer.
![Image](https://github.com/user-attachments/assets/10afe9c3-6d01-4f99-a21c-612a70d13044)

After running LaZagne, LimaCharlie's sensor will generate a NEW_PROCESS event and display its event information.
![Image](https://github.com/user-attachments/assets/65c7b70c-fd23-4757-a947-d6e9b57ef656)
***
Next, we'll move onto our detection & response rule creation.

Navigate to Automation -> D&R Rules -> New Rule.
![Image](https://github.com/user-attachments/assets/4040dfe0-42ad-4689-a80e-1a1a23c6bb24)

From here, we'll use a template to help create the detection rule.
Navigate to Automation > D&R Rules > windows_process_creation/proc_creation_win_lolbin_device_credential_deployment
![Image](https://github.com/user-attachments/assets/5d7997c3-af4f-4bce-99f6-2b82e697bb4e)

Under the D&R rule, click on the GitHub repository.
![Image](https://github.com/user-attachments/assets/989a4add-29ec-4fbf-a342-330afb201a42)

Select Raw and copy the rule content.<br>
![Image](https://github.com/user-attachments/assets/1eec9876-c776-43c4-8481-12552e4052a1)

Now, we'll look to modify both the detect and respond rules so that we'll detect our specific LaZagne attack. For now, we'll have our response just to be report and not yet isolate the machine.
![Image](https://github.com/user-attachments/assets/3af4edc0-235f-41d6-a9f0-3e2d0483123b) ![Image](https://github.com/user-attachments/assets/1c536c28-2e9b-4879-a586-3a479516b308)

And then we'll go ahead and test the rule by copying the event from our timeline, then paste it into the event search below our rule customization.
![Image](https://github.com/user-attachments/assets/efebaab7-b910-4e75-b17e-52e04b6f6e1e)
![Image](https://github.com/user-attachments/assets/91a0f962-b1eb-4b2a-8452-a7722aa22a43)

Great! Now we've just confirmed that our rule is working and is ready to deploy.
***
# Slack & Tines Setup
![Image](https://github.com/user-attachments/assets/274eeb80-88e8-4ca2-8b4e-c155c5b69d9a)
***
Navigate to [Slack](https://slack.com) and create a user account. Then create a new workspace for this project.
![Image](https://github.com/user-attachments/assets/38d48aff-0e0c-4430-9e97-f787ff7aacf1)

After choosing the free plan, let's create a new channel and name it Alerts. This will be the channel that Tines will send over our automated alerts to in regards to our sensor. <br>
![Image](https://github.com/user-attachments/assets/88173de3-7145-4ae7-b961-a7255489d372) ![Image](https://github.com/user-attachments/assets/69657e87-b6bb-41c9-becb-95e63a14c8d0)

***
![Image](https://github.com/user-attachments/assets/0fb37021-ae2c-4e01-9e11-2616ed2b3d06)




Navigate to [Tines](https://tines.com) and create a user account. Referrencing back to our flow diagram, we eventually want to integrate and link LimaCharlie and tines. Grab the Webhook icon from the tools section and drag it to the story. Name the Webhook Retrieves Detections and Description Retrieves Detections from LimaCharlie. Copy the Webhook URL and go back over to LimaCharlie.
![Image](https://github.com/user-attachments/assets/2506e48a-a009-49d6-83ab-23bbcdf1ac2e)

Back in LimaCharlie, go to Outputs -> Add Output -> Detections -> Tines.
![Image](https://github.com/user-attachments/assets/687b3296-4a90-4f95-847b-9c3080597894)

![Image](https://github.com/user-attachments/assets/b926f6f8-36ca-4278-a60c-e9eb0580e575)

![Image](https://github.com/user-attachments/assets/ca2ef056-fee6-40c7-9475-bbd13cce0d9f)

From here, let's name our Output and paste the webhook URL. Keep in mind that here, when saving the output, we will not see a detection yet. We'll need to run a command in our machine, refresh our samples, and then a sample detection will display afterwards.
![Image](https://github.com/user-attachments/assets/a4826de2-5bf3-4da6-ab91-94c2cec16330)
![Image](https://github.com/user-attachments/assets/ee89b581-0310-4250-9a40-5724d0f755bd)

Back in Tines, under Retrieve Detections, select Events and we'll open up our most recent populated sample event.
![Image](https://github.com/user-attachments/assets/54d56465-adf7-4b26-ba11-1ff0deba3eb3)


At this point, we'll seek to finally link together Tines and Slack.
From within Slack, choose More -> Automations -> Tines -> Add -> Add to Slack. Then follow steps for installation.  <br>
![Image](https://github.com/user-attachments/assets/c7ad2232-e049-4264-ac1d-725bb57be5d5)

Back over in Tines, navigate to Your Teams -> Credentials -> New Credentials -> Slack -> Use Tine's app for Slack.
![Image](https://github.com/user-attachments/assets/b8da1d5e-6eb1-41a7-8e9e-0b0d4366a9fa)

![Image](https://github.com/user-attachments/assets/03b9159f-7acd-4505-9755-bf1e2d6527a6)

![Image](https://github.com/user-attachments/assets/8185dfed-3da9-462d-909b-9bd0212ea927)

![Image](https://github.com/user-attachments/assets/ae7cccbf-11b1-4d95-8320-96865045c1b7)

![Image](https://github.com/user-attachments/assets/4a19d6f2-bcdf-4621-9950-833eccbb952d)

Now under Templates in Tines, select: Slack -> Send a Message

In Slack, right-click the #alerts channel -> View Channel Details > Copy Channel ID
![Image](https://github.com/user-attachments/assets/e8e8b0fc-7983-4746-bb31-95e9f337c2dc)

Paste the Channel ID into Tines.

# Tines Workflow Build

We can finally get started on building our diagram workflow. In Tines, grab the Webhook Detections action as well as Slack. We'll connect them together to build the logic flow.

Now, build the detection message using important fields from Retrieve Detections.<br>
![Image](https://github.com/user-attachments/assets/cf359135-1c09-48fc-b07a-0976f5fe90f1)
![Image](https://github.com/user-attachments/assets/d4050347-f76f-48bc-9bc0-857f977073aa)

Click Run to send a test message to Slack. Confirm the message appears in Slack.
![Image](https://github.com/user-attachments/assets/c30eb8a5-e94a-4f08-b526-4ec432e7efad)

Great! Now that we've confirmed the conection between Tines & Slack, let's also try sending email notifications. For our emails, I'll be using a disposable public email service through [SquareX](https://public.sqrx.com/web/). 
![Image](https://github.com/user-attachments/assets/e9124afc-ad09-4304-9382-b242f2c7a351)

In Tines, we'll add an action to the playbook by dragging over the "Send Email" action. We'll input our disposable email we've gotten from SquareX as the recipient. Then we'll test to see if we can properly receive the email. <br>
![Image](https://github.com/user-attachments/assets/88e0a8c7-06d4-4dd5-bbf3-7f371a6520be)
![Image](https://github.com/user-attachments/assets/2aa8319c-466c-4613-bf98-571aa8642699)
***

Awesome, we've confirmed the Tines & Slack connection, as well as test emails. Now we can configure our automated messages to list out our incident information. Let's refer to our intended information fields from our initial flow diagram. We can find these subject fields when going into our detection events in Tines, expanding Retrieve Detections -> Body -> Detect -> Event. Once we've located all our interested fields, we'll copy the path and paste into a notepad for reference.
![Image](https://github.com/user-attachments/assets/938ab4d3-207d-40ac-a64d-37fd6616a433)

After grabbing those paths, as long as we have an event detection retrieved within our playbook, we should be able to receive our desired information via Slack/email. If there is no detections at this point in Tines, I suggest that you rerun LaZagne.exe on this project's machine to detect an event. For our automations to trigger, Select Run -> Retrieve Detections -> Test.
![Image](https://github.com/user-attachments/assets/9f58b0b7-11c1-4947-8563-f0754c60ea08)

Let's grab our subject fields and their corresponding paths from earlier and paste them into the message fields of our Slack/email actions. In Slack, our alerts channel should look like the picture below, same with our email.
![Image](https://github.com/user-attachments/assets/b7a13faa-bc88-4a89-8e00-be12df9f61f0)

![Image](https://github.com/user-attachments/assets/3b4b19bb-7543-4b75-8245-95e59b829b2e)

![Image](https://github.com/user-attachments/assets/f23017ba-2e7f-43eb-b081-508ba59d7c71)

Sweet! We've gotten our desired incident information. Although our email formatting is a bit off. To fix this, I realized that the email is sent in HTML. So in Tines, select our "Send Email" action, under Body, find "Open HTML Editor." Here, we'll add break lines <br> to tidy up the formatting for our email.
![Image](https://github.com/user-attachments/assets/5006bb11-0fcd-4548-b135-7223535f7e79)
***

Onto our user prompt portion of the project, this is the webpage our analyst would visit in the event that they decide to isolate the machine based upon the information given regarding the incident(s). To find our user prompt in Tines, select tools, add a new page. Then Provide a Name, Description, and Success Message users will be seeing. 
![Image](https://github.com/user-attachments/assets/dd280f4d-edac-46a9-ad00-173a66edf647)

Let's find a Boolean under Page Elements that we can add into our user prompt. This will allow our analysts to isolate the affected machine. In addition, we'll edit our user prompt and input our detection subject fields from before. <br>
![Image](https://github.com/user-attachments/assets/9c62e277-8cfc-4bc7-ba4a-5774326c32e4)

![Image](https://github.com/user-attachments/assets/447f299d-fe8b-44ca-96b6-6f6c8c9f743a)

Awesome, our user prompt shows our information now.<br>
![Image](https://github.com/user-attachments/assets/a472e7c5-4db1-45d8-a0c6-a00e65d1c0d1)
***

Moving on, we can build upon our boolean responses and what Tines will do in the event of our Yes/No confirmation.

Starting off with No, let's select our Trigger tool, drag it onto our playbook, and name the tool "No." <br>
![Image](https://github.com/user-attachments/assets/c897aac5-458e-449a-9af5-b2629322f2af)

![Image](https://github.com/user-attachments/assets/5d99454b-72c1-4e97-85ac-99fbfc89a2d1)

Underneath that, we will add Slack and add the path "retrieve_detections.body.detect.routing.hostname." This path will simply grab the hostname of the affected machine and autofill it within our alert message. <br>
![Image](https://github.com/user-attachments/assets/251ba015-76bd-4cda-aa10-5499f3970846)

![Image](https://github.com/user-attachments/assets/7fe5dfca-7a5d-49ad-b3bb-335b00fabd93)

Let's try testing out our flow. We'll visit the User prompt page, click No. We should get a message from Slack asking us to investigate the machine.
![Image](https://github.com/user-attachments/assets/9a06dcd6-99a3-49b1-85f0-d61f030f89e5)

Sick, we've finished that half of our user prompt actions. Onto our Yes prompt, we'll grab a Trigger action again and name our fields. <br>
![Image](https://github.com/user-attachments/assets/9ee31cbc-0d4e-42d9-8300-4d7090154f61)

![Image](https://github.com/user-attachments/assets/289fef09-d8d0-4db1-86fb-af722c9a2105)

Instead of immediately connecting to Slack here, we'll add a LimaCharlie element. Select templates, type in LimaCharlie, and search for Isolate Sensor. We'll name it, and select URL -> Add value "retrieve_detections.body.routing.sid." SID standing for our sensor ID from LimaCharlie. <br>
 ![Image](https://github.com/user-attachments/assets/b9fa6c04-4e24-41a5-8153-4e5339f30ca5)

![Image](https://github.com/user-attachments/assets/cb21d343-b4ca-441f-bf2d-f646b7fc5978)

![Image](https://github.com/user-attachments/assets/fc2de532-8aa7-49d0-9cea-683577d66ade)

This is a tricky point in the project between to connect back this feature to LimaCharlie, we'll need the correct Credentials again similar to how we added one for Slack. When testing out our automation, if I did not configure a LimaCharlie credential, I received these error logs.
![Image](https://github.com/user-attachments/assets/0c863e26-ca4d-419c-b398-2f1df32956e7)

![Image](https://github.com/user-attachments/assets/57ed9f7a-67ee-4fd7-b297-f9e7186d1d6c)

So to fix this, we'll refer to the LimaCharlie documentaion for API authentication information.
![Image](https://github.com/user-attachments/assets/9b2ff483-0251-43ab-9da2-39e4a2082589)

Back in Tines, navtigate to Your Teams -> Credentials -> +New -> JWT. Then we'll name this credential LimaCharlie and add in our JWT value from LimaCharlie. This can be found back in the LimaCharlie dashboard -> Access Management -> REST API.
![Image](https://github.com/user-attachments/assets/2a19acdf-cc2c-46db-9b4f-6f941b1b3e76)

![Image](https://github.com/user-attachments/assets/f0668829-7087-40c1-95aa-5f8033d93894)

![Image](https://github.com/user-attachments/assets/b5a58ae5-a71d-4bf2-8742-ad5c90bfeabc)

Now that I added a new LimaCharlie credential, I'll make sure to replace it under Headers-> Bearer within my "Isolate Sensor" playbook element.
![Image](https://github.com/user-attachments/assets/26c60ae7-e505-4d4f-8cbc-35185b2d3cbb)

At this point, I ran another test and selected "Yes" as my user prompt response. As soon as I select, my machine should be isolated from any network access and I should receive alerts via Slack/email about its isolation status.
![Image](https://github.com/user-attachments/assets/b0265a1e-cba2-4dd8-b4be-a80ce28d3165)

![Image](https://github.com/user-attachments/assets/39c511dd-a604-4944-8410-3263aaaec94e)
Alright!!

To finish this up, let's grab our isolation status and send this over to Slack. Again, select templates, type in LimaCharlie, and search for Get Isolation Status. Make sure our credentials are set to the correct LimaCharlie configuration. 
![Image](https://github.com/user-attachments/assets/46ef86a5-3bd8-48cf-8e10-d070943b08e0)

Finally, we'll add a final Slack element for it to send us an automated alert to investigate our machine after isolation. Run our test and we should see our desired message.
![Image](https://github.com/user-attachments/assets/2cb51915-7f69-4296-af68-e3ad2f368be8)
***

Overall, here is our playbook looks in Tines.
![Image](https://github.com/user-attachments/assets/a78086ea-c817-4172-a06b-e18607733610)
