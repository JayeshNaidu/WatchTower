# WatchTower - Setup Guide

Welcome to the guide for setting up this this project environment using a **Kali Linux VM** and the **Elastic Web Portal**. 

## Prerequisites

Before starting, make sure you have the following:

- **Virtualization Software**: VirtualBox or VMware.
- **Linux Experience**: Basic familiarity with Linux commands and virtualization concepts.
- **Elastic Cloud Account**: Sign up for a free account [here](https://cloud.elastic.co/registration).
  
## Overview

In this guide, we'll cover the following tasks:

1. **Setting up a Free Elastic Account**: Sign up for Elastic Cloud and configure your deployment.
2. **Configuring the Elastic Agent**: Install and configure the Elastic Agent on your Kali Linux VM to forward security logs to the SIEM.
3. **Generating Security Events**: Simulate security events and attacks on your Kali VM to analyze the output in the SIEM.
4. **Querying and Analyzing Events**: Use Elastic SIEM to view and analyze the logs collected from your Kali VM.
5. **Creating a Dashboard**: Build a custom dashboard to visualize security events.
6. **Setting Up Alerts**: Configure alerts in Elastic Stack to notify you of suspicious activity.

## Setup Steps

### Step 1: Get your Elastic Cloud Account
- Go to the [Elastic Cloud Portal](https://cloud.elastic.co/) and sign up for a free account.
- Create a new deployment and take note of your **Elastic Cloud credentials**.

### Step 2: Setting Up Your Kali Linux VM
- Install **VirtualBox** or **VMware** and download a Kali Linux VM image.
- Boot up the VM and ensure you have network connectivity.

### Step 3: Installing Elastic Agent on Kali Linux
- Open a terminal in Kali Linux and follow the instructions to install Elastic Agent:
- Click the burger menu in the web portal, you will see "Add Integration" button on the left bottom.
- Select Elastic Defend, copy and run the code to download and install the elastic agent in Kali.
- Click continue to apply the relevant policy.

![Step 3](./images/1.%20Integrating%20Agent.png)

### Step 4: Generating Security Events

To test your Elastic Stack SIEM, we need to generate security events on your Kali Linux VM. Kali has several built-in tools that can simulate various types of attacks. Here is an example:

**Nmap**: Perform a port scan on a target host.
   ```bash
   sudo nmap -sS -p- <target_ip>
  ```

![Step 4](./images/3.%20running%20nmap%20for%20logs.png)

### Step 5: Querying and Analyzing Events in Elastic SIEM

- Once security events have been generated and collected, it's time to query and analyze them using the Elastic SIEM. This step will help you investigate the events captured by the Elastic Agent and uncover potential security threats.
- Click the burger menu in the web portal and find **logs**.
 - You can perform detailed searches using the query interface available in the **Events** section.
   - For example, to search for network activity from Nmap, use a query like:
     ```bash
     query: event.action: "nmap_scan" or process.args: "sudo"

![Step 5](./images/5.%20nmap%20argument%20seen.png)

### Step 6: Creating a Dashboard

In this step, we will create a custom dashboard in **Kibana** to visualize the security events collected by Elastic SIEM. A dashboard provides an overview of critical security data and helps in quick monitoring.

#### Steps to Create a Dashboard:

1. **Access Kibana Dashboards**:
   - In the Elastic Cloud Portal, go to the **Kibana** section.
   - From the left-hand panel, click on **Dashboard** and then select **Create new dashboard**.


2. **Add Visualizations**:
   -  Within the visualization editor located on the right-hand side, navigate to the “Metrics” section. Here, select “Count” as the vertical field type. This choice allows you to visualize the count of events over time effectively. For the horizontal field, opt for “Timestamp”.
     
   - You can customize the chart to display network traffic over time by selecting the **X-axis** as `@timestamp` and **Y-axis** as `Count of events`.
  
   - Click on save and set a name to your new visual.

![Step 6](./images/6.%20Create%20Dashboard.png)

### Step 7: Setting Up Alerts in Elastic SIEM

To proactively detect suspicious activity and respond in real-time, we can set up **alerts** in the Elastic SIEM. Alerts are triggered based on pre-defined conditions and can notify you via email or other communication channels when a potential security threat is detected.

#### Steps to Set Up Alerts:

1. **Navigate to Rules**:
   - In the **Detections** tab, locate the **Manage Rules** button in the top-right corner and click on it.
   - Here, you can see a list of pre-built detection rules, or you can create your own custom rules.

2. **Create a New Detection Rule**:
   - Click on **Create New Rule** to start building a custom alert.
   - You will be presented with a rule creation wizard where you can define the rule conditions and actions.

3. **Define Rule Criteria**:
   - Choose a **rule type** that suits your security needs. Some common types include:
     - **Threshold-based rule**: Trigger alerts when the number of specific events exceeds a certain threshold.
     - **Custom query rule**: Trigger alerts based on a specific query that matches your conditions.
   
   - For example, to detect multiple failed login attempts within a short time, you can create a query like:
     ```bash
     event.category: "authentication" AND event.outcome: "failure"
     ```
   - Set the threshold condition to trigger the alert if **more than 5 failed login attempts occur within 10 minutes**.

4. **Configure Alert Actions**:
   - After defining the rule conditions, you need to configure **actions** that will take place when the rule is triggered.
   - Actions can include:
     - **Sending an email** notification to your security team.
     - **Logging the alert** for further analysis.
     - **Webhook integration** to send alerts to third-party tools like Slack, PagerDuty, or custom APIs.
   
   - For an email notification, select **Add Action**, choose **Email**, and configure the recipient details.

5. **Activate the Rule**:
   - Once all the rule conditions and actions are set, review the rule and click **Activate** to enable it.
   - Your alert is now live, and you will receive notifications whenever the conditions you defined are met.

![Step 7](./images/7.%20Created%20Nmap%20Alert.png)

### Alert Results:

The Below image shows that we have recieved alerts which triggered alert emails for it.

![Step 8](./images/8.%20Alert%20recieved.png)

## Thats It !! You now have your own Elastic SIEM at home.









  

