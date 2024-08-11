# Splunk Lab (TryHackMe)

## Overview
I did this lab as part of the TryHackMe SOC Level 2 course. The primary focus was on installing, configuring, and utilizing Splunk for log management and analysis on both Linux and Windows operating systems. The lab also included creating dashboards, reports, and custom data manipulation using Python scripts.

## Installation and Configuration of Splunk

### Linux
- **Downloading and Uncompressing Splunk**: I downloaded and uncompressed the Splunk package for installation.
<img width="733" alt="image" src="https://github.com/user-attachments/assets/352e0ad0-0e0a-4e10-9f3a-b41b9080950b">

- **Accessing Splunk**: After the installation, I accessed the Splunk web interface to start configuring it.
   ![Pasted Graphic 14](https://github.com/user-attachments/assets/3964afbd-8017-4bae-9ece-3ee8c95ddf2d)
- **Installing Splunk Forwarder**: I installed the Splunk Forwarder and moved it to the `/opt/` directory.

    Installing the Splunk forwarder <img width="1327" alt="image" src="https://github.com/user-attachments/assets/cf859b10-a3bc-46f6-9233-3082892f7d81">
- **Configuring Listening Port**: Configured Splunk to listen on port `9997` to receive data from the forwarders.
 ![Pasted Graphic 18](https://github.com/user-attachments/assets/b1ab1e4f-574f-45d8-b453-3068b7341158)
- **Creating an Index**: I created an index  in Splunk to store all incoming data from the forwarders.
 ![Pasted Graphic 19](https://github.com/user-attachments/assets/5deed616-10b2-451c-8177-ec5dafcd3b3f)
- **Configuring Forwarder**: I configured the forwarder to ensure it sends data to the right destination by going to the `/opt/splunkforwarder/bin` directory and typing in the command: `./splunk add forward-server 10.10.128.83:9997`

- **Monitoring `syslog` with Splunk Forwarder**: Configured Splunk Forwarder to monitor the `/var/log/syslog` file, enabling the ingestion of system logs into Splunk's `Linux_host` index. 
  ```bash
  ./splunk add monitor /var/log/syslog -index Linux_host
  

### Windows
- **Administrator Account Creation**: Created an administrator account for Splunk and installed it on a Windows machine.
- **Configuring Data Reception**: Set up Splunk to receive data from the forwarder on port `9997`.
  <img width="1512" alt="image" src="https://github.com/user-attachments/assets/caeb9fad-04ab-42b4-80f1-120b2969fc6c">
  
- **Installing Splunk Forwarder**: The Splunk Forwarder was installed and configured on the Windows machine.
<img width="1471" alt="image" src="https://github.com/user-attachments/assets/5cda55f0-bc12-4874-8ccd-ad2b0c435a17">
  
- **Listener Setup on Splunk Forwarder**: Configured the forwarder to send logs to the main Splunk instance.
 ![Pasted Graphic 27](https://github.com/user-attachments/assets/5eb6cbc6-9b80-47e9-a93d-b809ee847ad3)

## Dashboards and Reports

### VPN User Logins Analysis
- **Querying VPN Logins**: Used SPL (Search Processing Language) to determine the number of times each VPN user logged in during a specified time window.
  ![Pasted Graphic 32](https://github.com/user-attachments/assets/68b03c3d-41a0-4ccf-b9d0-43d52bd73b92)
  
- **Automating the Query**: Automated the above query for continuous monitoring.
 ![Pasted Graphic 33](https://github.com/user-attachments/assets/e73e79ac-428a-45e9-9a21-15ad73ff12e2)
- **Dashboard Creation**: Created a dashboard to visualize the VPN login data.
 ![Pasted Graphic 34](https://github.com/user-attachments/assets/5049b63d-cabd-41c2-99a6-03f9b6428dc8) 
- **Adding Report Results to Dashboard**: Integrated the query results into the dashboard for easy access and monitoring.
 ![Pasted Graphic 35](https://github.com/user-attachments/assets/dea87d8a-58b8-45fd-8972-bf410f5ea1f7)

## Data Manipulation and Parsing

### Learning Objectives:
- Understanding how events are parsed in Splunk.
- Importance of configuration files like `inputs.conf`.
- Extracting custom fields for filtering.
- Identifying timestamps in event logs.

### Creating and Configuring Apps
- **Creating an App**: Splunk apps are pre-packaged software modules or extensions that enhance the functionality of the Splunk platform.  The purpose of Splunk apps is to provide specific sets of features, visualizations, and configurations tailored to meet the needs of various use cases and industries. I created a Splunk app to manage and visualize data.
 <img width="758" alt="Pasted Graphic 7" src="https://github.com/user-attachments/assets/8da75493-3c4a-48d2-9024-d1bbc90d7e63">
 
- **Python Scripting**:: Created a simple Python script (`samplelogs.py`) and ran it 
  <img width="1125" alt="image" src="https://github.com/user-attachments/assets/65970fa9-854d-4840-a887-fd3f2c89cd7c">
  ![image](https://github.com/user-attachments/assets/744b7086-8a44-453a-a137-cc7690a6a151)

- **Creating Inputs.conf**: Created a configuration `inputs.conf` to pick out from the script (`samplelogs.py`) and send it to Splunk with the index `main` every 5 seconds.
![image](https://github.com/user-attachments/assets/d6fc62ec-32bb-4385-9d72-515b36a42162)


- **Log Analysis**:  Reviwed the logs within Splunk.
<img width="1508" alt="Pasted Graphic 1" src="https://github.com/user-attachments/assets/10439df4-bfd7-49b5-a78a-7588221aa11b">

### Custom VPN Log Handling
- **Simulated Client VPN Logs**: Created a script to simulate a custom VPN application that generates logs with user, server, and connection action details.
- ![image](https://github.com/user-attachments/assets/73dcb615-bb31-40b6-ab8d-90551808db88)
- The above lines tell Splunk to run the script `vpnlogs` every 5 seconds and send the output to the `main` index with sourcetype `vpn_logs` and host value as `vpn_server`.

- Viewed the VPN logs in splunk, but there was a problem since Splunk could not determine the boundaries of each event and considered multiple events as single events.
  ![image](https://github.com/user-attachments/assets/a1176fc6-0a6d-427e-b389-96b01789d321)
- To fix the problem, I made changes to the `props.conf` file. I created a regex pattern to break events after disconnect or connect.
![image](https://github.com/user-attachments/assets/628507d8-5461-443f-819f-948b9ce244a4)
- I restarted splunk and the changes were applied to the VPN logs.
  ![image](https://github.com/user-attachments/assets/16cd2107-ed1b-46c9-b431-f046181f9db3)
## Parsing Multi-line Events


## Conclusion
This lab exercise provided hands-on experience in configuring and managing Splunk for both Linux and Windows environments. I learned how to effectively use Splunk's SPL, configure forwarders, create dashboards, and manipulate data using custom scripts and regular expressions.

