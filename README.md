## FUTURE_CS_03

## HELLO 😀
## Here is the walkthrough documentation of how I completed Task 3 - the final task of my Cyber Security Internship at Future Interns :-

---

# Task 3 : **Analyze and Respond to a Simulated Cybersecurity Incident**
![image](https://github.com/user-attachments/assets/b6076efc-b272-4843-9e47-b63c8891cf9b)
---

## Steps For using Wireshark :-

1. Setup Wireshark:-
- Wireshark is a network protocol analyzer that lets you capture and interactively browse traffic running on a computer network.
To install Wireshark, follow the steps for your operating system:

- Linux: Run sudo apt install wireshark.

- Windows/MacOS: Download it from the official Wireshark website and follow the installation instructions : https://www.wireshark.org/download.html
---
2. Capturing Network Traffic:-
- Launch Wireshark and select the network interface you want to capture traffic on (e.g., eth0, wlan0).
- Start capturing by clicking the blue shark fin icon.
- Allow the capture to run for some time or until you see enough traffic for analysis.
- Stop capturing by clicking the red square icon.
---
3. Loading the Capture for Analysis:-
- If you’ve saved a previous capture, you can load it by going to File > Open and selecting your .pcap or .pcapng file.
- For the walkthrough, let’s assume you’ve loaded a capture file. [ I just scanned the OWASP JUICE SHOP Website for example and saved the pcap file afterwards ]
---
4. Filter and Identify Suspicious Traffic:-
- Start with common Wireshark filters to find interesting traffic. Use the display filter to narrow down the traffic you're analyzing:
- TCP traffic on port 1514: tcp.port == 1514
- TLS traffic on port 443: tcp.port == 443
- TCP reset traffic: tcp.flags == 0x04
- In this case, we found suspicious traffic to IP 103.162.246.81 on non-standard port 1514, which could be related to malware.

              tcp.port == 1514 && ip.addr == 103.162.246.81

- This filter will show only the traffic between the local host and the suspicious IP address 103.162.246.81.
---
5. Analyzing Suspicious Traffic:-

- Check Packet Details
- For each suspicious packet, click on it to see the detailed breakdown in the middle pane.
- Look for data exchanged, unusual flags, or non-standard port usage.

- Investigate the IP
- After identifying traffic to IP 103.162.246.81, perform an OSINT investigation using tools like VirusTotal or abuseIPDB to check if this IP has been flagged for malicious activity.
---
6. Analyzing TLS Traffic:-
- To investigate encrypted traffic
- Use the following filter to isolate HTTPS traffic : tls
- Analyze the destinations and the volume of the encrypted traffic to ensure it's legitimate. In this case, multiple external IPs were communicating with the host, which could be either normal web browsing or encrypted command-and-control (C2) communication.
---

7. Understanding TCP Reset Packets:-
- TCP resets (RST flags) are used to abruptly terminate a connection. Analyze the source and destination of these packets.
- In our case, the reset packet (RST flag) was sent from 52.26.51.69:443 to 192.168.29.249, which could indicate connection rejection or network scanning.
---
8. Root Cause Analysis:-
- Suspicious Activity on Port 1514: Non-standard ports can indicate custom applications or malware C2 servers.
- Ambiguous TLS Traffic: Encrypted traffic might be normal or indicative of data exfiltration.
- The key suspicion here was the unusual traffic to IP 103.162.246.81 on port 1514. Further investigation is required to determine if this traffic is malicious.
---
9. Recommendations for Mitigation:-
- Enhanced Monitoring: Ensure that network traffic is continuously monitored using tools like Splunk, Kibana, or SIEM solutions.
- Deploy Intrusion Detection Systems (IDS/IPS): Set up both network and host-based detection systems to alert on unusual traffic, such as non-standard port usage.
- Strengthen Endpoint Security: Ensure that all systems have up-to-date antivirus software and endpoint detection and response (EDR) tools.
- Improve Firewall Rules: Review and limit outbound traffic to non-standard ports.
- Implement Security Awareness Training: Train employees to recognize phishing, malware, and other common cyber threats.
---
10. Conclusion:-
- This analysis was a preliminary investigation into suspicious network traffic involving a non-standard port and encrypted communication. The next steps would involve deeper packet capture analysis, checking system logs, and strengthening monitoring across the network.

---

## Steps For using Splunk :-

### 1. Setup:-
### Prerequisites:-
- Splunk installed on your Windows OS.
 Download Splunk : https://www.splunk.com/en_us/download.html
---
- Kaggle's Intrusion Detection Dataset downloaded and unzipped.
Download Dataset : https://www.kaggle.com/datasets/sampadab17/network-intrusion-detection?resource=download
---
- Use only the train_data.csv file from the unzipped folder.
---
### 2. Upload Dataset to Splunk

1. Log in to Splunk:

2. Open Splunk by entering localhost:8000 in your browser.

3. Log in using your Splunk credentials.
Upload the Dataset (train_data.csv):

4. Navigate to Settings > Add Data.

5. Choose the option Upload File.

6. Click Select File and upload the train_data.csv file.

7. For Source Type, select csv (or Splunk may auto-detect).

8. In the Set Source Type step, you can review the fields.

9. Click Next and complete the upload by clicking Start Searching.

---

### 3. Detect Failed Login Attempts:-
- Search Query:

We’ll detect failed login attempts by identifying events with more than three failed login attempts or where logged_in = 0.

      index=_internal OR index=* num_failed_logins>3 OR logged_in=0 | stats count by host

- Go to Search & Reporting in Splunk.
- In the search bar, paste the above query.
- Click Search.
- You’ll see the hostnames with more than three failed login attempts or where users are not logged in.
---
### 4. Detect Unusual Data Transfers:-
- Search Query:

We’ll identify suspicious data transfers by looking for large data transfers.

     index=_internal OR index=* src_bytes>1000000 OR dst_bytes>1000000 | stats sum(src_bytes) as total_src_bytes, sum(dst_bytes) as total_dst_bytes by host

- Stay on the Search & Reporting page.
- Paste the query into the search bar and hit Search.
- The results will display hosts where large amounts of data were transferred (both incoming and outgoing).
- Review any anomalies or unusual activity in the data transfers.
---
### 5. Data Visualization:-
- You can create quick visualizations to make the data easier to interpret. Here's how you can create visualizations for both failed login attempts and unusual data transfers.

- Steps to Create a Bar Chart:
1. After running any of the queries above, click the Visualization tab next to the Statistics tab.
2. Choose Bar Chart (or any other chart type that fits your needs).
3. You can save this visualization by clicking Save As > Report.
---
### Conclusion:-
By following the above steps, you should be able to:

1. Detect failed login attempts based on the number of failed login attempts or unsuccessful logins.
2. Detect large or unusual data transfers.
3. Visualize the results using Splunk's built-in charting tools.

---

## Screenshots :-

-  Screenshot Highlighting Suspicious Port 1514 Traffic:

 ![Screenshot 2025-02-19 114449](https://github.com/user-attachments/assets/7b42035d-ba98-454c-9361-084bdaa8e67d)

 - Suspicious Port 1514 Traffic: This screenshot highlights the unusual TCP communication between 192.168.29.249 and 103.162.246.81 on port 1514 (Packets 1,2, 26, 27). Port 1514 is not a standard service port and this traffic warrants further investigation.

---
 
 -  Screenshot Showing TLS (HTTPS) Traffic on Port 443:

   ![Screenshot 2025-02-19 115130](https://github.com/user-attachments/assets/7152ece1-9809-43a0-8c01-ee48534be813)

-  TLS (HTTPS) Traffic on Port 443:This screenshot shows examples of the TLSv1.2 "Application Data" packets (e.g., Packets 10, 11, 33, 34) observed in the capture.While HTTPS is common, the volume and destinations require further analysis to determine if it's related to normal web browsing or potentially malicious activity 

---

-- Screenshot Showing TCP reset traffic:

  ![Screenshot 2025-02-19 115518](https://github.com/user-attachments/assets/c2ca3eca-daf6-4cfe-839e-5d0e5ca44e12)

- TCP Reset (RST) Packet: Packet 6 in the capture is a TCP Reset (RST) packet sent from 52.26.51.69:443 to 192.168.29.249:53330. This indicates a TCP connection reset,which could be due to a connection rejection or network issue and needs to be considered in the analysis

---


