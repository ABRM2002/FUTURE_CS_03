## FUTURE_CS_03

## HELLO 😀
## Here is the walkthrough documentation of how I completed Task 3 - the final task of my Cyber Security Internship at Future Interns :-

---

# Task 3 : **Analyze and Respond to a Simulated Cybersecurity Incident**
![image](https://github.com/user-attachments/assets/b6076efc-b272-4843-9e47-b63c8891cf9b)
---

## Steps For using Wireshark :-





















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

   
