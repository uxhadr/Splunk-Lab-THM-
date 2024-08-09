# Splunk Lab (TryHackMe)

## Overview
I did this lab as part of the TryHackMe SOC Level 2 course. The primary focus was on installing, configuring, and utilizing Splunk for log management and analysis on both Linux and Windows operating systems. The lab also included creating dashboards, reports, and custom data manipulation using Python scripts.

## Installation and Configuration of Splunk

### Linux
- **Downloading and Uncompressing Splunk**: I downloaded and uncompressed the Splunk package for installation.
<img width="733" alt="image" src="https://github.com/user-attachments/assets/352e0ad0-0e0a-4e10-9f3a-b41b9080950b">

- **Accessing Splunk**: After the installation, I accessed the Splunk web interface to start configuring it.
-\ ![Pasted Graphic 14](https://github.com/user-attachments/assets/3964afbd-8017-4bae-9ece-3ee8c95ddf2d)
- **Installing Splunk Forwarder**: I installed the Splunk Forwarder and moved it to the `/opt/` directory.

    Installing the Splunk forwarder <img width="1327" alt="image" src="https://github.com/user-attachments/assets/cf859b10-a3bc-46f6-9233-3082892f7d81">
- **Configuring Listening Port**: Configured Splunk to listen on port `9997` to receive data from the forwarders.
 ![Pasted Graphic 18](https://github.com/user-attachments/assets/b1ab1e4f-574f-45d8-b453-3068b7341158)
- **Creating an Index**: I created an index  in Splunk to store all incoming data from the forwarders.
- ![Pasted Graphic 19](https://github.com/user-attachments/assets/5deed616-10b2-451c-8177-ec5dafcd3b3f)
- **Configuring Forwarder**: I configured the forwarder to ensure it sends data to the right destination by going to the `/opt/splunkforwarder/bin` directory and typing in the command: `./splunk add forward-server 10.10.128.83:9997`

- **Monitoring `syslog` with Splunk Forwarder**: Configured Splunk Forwarder to monitor the `/var/log/syslog` file, enabling the ingestion of system logs into Splunk's `Linux_host` index. 
  ```bash
  ./splunk add monitor /var/log/syslog -index Linux_host
  
### Windows
- **Administrator Account Creation**: Created an administrator account for Splunk and installed it on a Windows machine.
- **Configuring Data Reception**: Set up Splunk to receive data from the forwarder on port `9997`.
- **Installing Splunk Forwarder**: The Splunk Forwarder was installed and configured on the Windows machine.
- **Listener Setup on Splunk Forwarder**: Configured the forwarder to send logs to the main Splunk instance.

## Dashboards and Reports

### VPN User Logins Analysis
- **Querying VPN Logins**: Used SPL (Search Processing Language) to determine the number of times each VPN user logged in during a specified time window.
- **Automating the Query**: Automated the above query for continuous monitoring.
- **Dashboard Creation**: Created a dashboard to visualize the VPN login data.
- **Adding Report Results to Dashboard**: Integrated the query results into the dashboard for easy access and monitoring.

## Data Manipulation and Parsing

### Learning Objectives:
- Understanding how events are parsed in Splunk.
- Importance of configuration files like `inputs.conf`.
- Extracting custom fields for filtering.
- Identifying timestamps in event logs.

### Creating and Configuring Apps
- **Creating an App**: Developed a custom Splunk app to manage and visualize data.
- **Python Scripting**: Created a Python script (`samplelogs.py`) to generate sample logs, which were sent to Splunk with the index `main` every 5 seconds.
- **Log Analysis**: Reviewed and analyzed the logs within Splunk.

### Custom VPN Log Handling
- **Simulated Client VPN Logs**: Created a script to simulate a custom VPN application that generates logs with user, server, and connection action details.
- **Log Collection and Parsing**: Configured the system to send these logs to the main index with specific source types and host values.
- **Regex Parsing**: Utilized regular expressions to parse and structure the VPN logs for detailed analysis.
- **Custom Log Aggregation**: Combined multiple log entries into single events for streamlined analysis, especially for logs starting with "authentication."

## Conclusion
This lab exercise provided hands-on experience in configuring and managing Splunk for both Linux and Windows environments. I learned how to effectively use Splunk's SPL, configure forwarders, create dashboards, and manipulate data using custom scripts and regular expressions.

