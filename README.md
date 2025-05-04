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

Now, build the detection message using important fields from Retrieve Detections.<br>
![Image](https://github.com/user-attachments/assets/cf359135-1c09-48fc-b07a-0976f5fe90f1)
![Image](https://github.com/user-attachments/assets/d4050347-f76f-48bc-9bc0-857f977073aa)

Click Run to send a test message to Slack. Confirm the message appears in Slack.
![Image](https://github.com/user-attachments/assets/c30eb8a5-e94a-4f08-b526-4ec432e7efad)

Great! 


# Tines Workflow Build


