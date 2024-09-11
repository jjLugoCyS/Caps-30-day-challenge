# Caps-30-day-challenge

#Day 1
 1. <a href="https://app.diagrams.net/">Draw.io</a> diagram mapping out the challenge with components, machines, and funtions. Six (6) servers and 2 Host machines with connections, functions, and IP addresses. Connect the Windows and Ubuntu Servers to the Fleet Server. The Fleet Server connects to the Elastic & Kibana Servers for bidirectional communication. Connect the osTicket Server to the E&K Server with a bidirectional connection as well. Connect the Windows and Ubuntu Servers to the E&K Server that connection represents the logs being forwarded from the servers. Map out the Private Network Range. Add in the internet gateway and connect it to a cloud (representation of the internet) and connect the gateway to the VPC. A desktop computer connected to the internet and anothe computer, the attacker machine w/ Kali Linux (the C2 server being mythic), both connected to the internet.<br>
 ![30day-Caps-Diagram](https://github.com/user-attachments/assets/9a97c6c7-ccb6-459b-bbb1-53bf8d69a188)<br>
*Ref 1: Diagram*

#Day 2
 1. ELK Stack intorduction. Elasticsearch, Logstash, Kibana. Elasticsearch stores logs and allows for searching through your data. Logstash collects telemetry as well as filter through that telemetry to pick and choose what data you want to parse. Kibana queries logs for visualizations to build dashboards to search for certain data and create visualizations, reports, and alerts. There are five (5) big benefits for using the ELK Stack.
![ELK Stack MYDFIR](https://github.com/user-attachments/assets/a377c5c9-100a-4f77-9d6d-747c7c5d01df)<br>
*Ref 2: ELK Stack*<br>
 ![5 ELK benefits](https://github.com/user-attachments/assets/233fe37b-3d92-4eef-8f76-5a94f464cf9b)<br>
*Ref 3: ELK Benefits*

#Day 3
 1. Setup Elasticsearch. Using <a href="https://www.vultr.com/">Vultr</a> create a Virtual Private Cloud (VPC) network. Select a location > add an IP range > name your VPC.
 2. Deploy a new server: Optimized cloud compute > choose location > select Ubuntu 22.04 for Elasticsearch > 80GB NVME > disable: autobackups, IPv6. enable: VPC 2.0 and make sure to choose the one you created > name the host > click deploy new.
 3. Open PowerShell and SSH into the server. Using username (root) and the public IP, use password given on your vultr page. Update repositories.
 4. Install <a href="https://www.elastic.co/downloads/elasticsearch">Elasticsearch debX86_64</a> > copy the link address from the download button > use the wget command in PS to download it.
 5. Copy the security information and paste it in notepad for later. Open the elasticsearc.yaml and change the network.host to the public IP address and uncomment http.port.
 6. Back at Vultr go to firewall settings > add a firewall group > change SSH to allow MYIP. Navigate to your VM settings and select the newly created firewall rule.
 7. Start Elasticsearch.<br>
 ![elasticsearch start](https://github.com/user-attachments/assets/401f90ab-973b-4f9d-aa73-8d173c1a6214)
*Ref 4: Elasticsearch Start*

#Day 4
 1. Download <a href="https://www.elastic.co/downloads/kibana">Kibana</a> using the same procedure as Elasticsearch.
 2. Configure Kibana "nano /etc/kibana/kibana.yaml", uncomment server.port and server.host along with the public IP of your server on server.host .
 3. Run "systemctl daemon-reload", "systemctl enable kibana.service", "systemctl start kibana.service", and "systemctl status kibana.service" in that order.
 4. Run "./elasticsearch-create-enrollment-token --scope kibana" and copy the token to notepad.
 5. Open a new browser window with the server public IP address and port 5601, do this after adjustin firewall rules to allow your ip address access. on the server firewall "tcp 1-65535 MYIP" and "ufw allow 5601" on the server itself.
 6. In the elastic config read through the instructions and open your suroconfig security information notes.
 7. Add in your keystores: In your ssh session invoke kibana encryption keys. Copy these keys to your notes and invoke Kibana keystore > add a field name > then the encryption value. DO this for all 3 fields from the encryption keys. Use systemctl restart kibana.service to restart the server. Open your web browser and everything should be ready. 
 ![Kibana status](https://github.com/user-attachments/assets/16ec1a4c-2114-4cd1-b0ad-114aeafe6793)<br>
 *Ref 5: Kibana.service status*<br>
![kibana alerts](https://github.com/user-attachments/assets/c7c59172-618d-445b-94c4-88f6499849cb)<br>
*Ref 6: Kibana alerts*<br>

#Day 5
 1. Log into Vultr and deploy a new server > select cloud compute-shared cpu > choose a location > select Windows server 2022 standard image > choose a 55GB SSD > diable autoback ups and IPv6 (the diagram was adujusted to remove the Windows and Ubuntu servers from th VPC in case a comprise) > enter a hostname > click deploy.
 2. Click on the server, select view console to spin up the server.
 3. Copy the public IP and paste it into the remote desktop computer field.
    ![30days-Caps-Diagram](https://github.com/user-attachments/assets/6d70f37d-f77d-4509-b6b5-19034233d710)<br>
    *Ref 7: Updated diagram*<br>
    ![Windows 2022 server](https://github.com/user-attachments/assets/96fef5cf-9f3e-4bd6-ac58-331dee1e0bf2)
    *Ref 8: Windows 2022 server*<br>

#Day 6
 1. Elastic agents and Fleet server. Stand alone fleet server or Fleet Managed serve. Beats vs. Agents. Ultimately Fleet Servers localize Agents.
    ![types of beat](https://github.com/user-attachments/assets/b67e4d5a-f6f3-47f6-a437-276dd7dff937)<br>
    *Ref 9: Types of Beats*<br>

#Day 7 
 1. Create a Fleet Server: Deploy > New Server > Dedicated CPU > location > Ubuntu 22.04 > 30GB NVMe > VPC 2.0 only additional feature > hostname > deploy now.
 2. On the Elastic WebGUI: Click Fleet > add a Fleet server, enter https: and then the servers public IP > Generate fleet server policy > Copy Installation output.
 3.  On Vultr click the fleet server and view console to check if the server is ready > open PowerShell and SSH into the server > apt-get update && apt-get upgrade -y > copy/paste installation for fleet server > In Vultr Fleet server go to the firewall to allow all in port ranfe 1-65535 custom and your fleet IP address > modify the firewall rule on your ELK server with "ufw allow 9200" > clcik continue enrolling Elastic agent > enter name > create policy > select Windows tab and copy > On Vultr go to the Windows server > Open PowerShell and paste the Windows copy > modify in fleet server firewall if you get a connecting error > In Elastic WebGUI click fleet > edit fleet server yo change port > ,odify installation URL with the new port 8220 and include "--insecure".
    ![Fleet Logs](https://github.com/user-attachments/assets/db6e0c35-865c-4e85-8bfb-8bbea90c3aa6)<br>
    *Ref 10: Fleet logs*<br>

#Day 8
 1. Sysmon is an Endpoint tool used for even monitoring. It generates telemetry to increase malicious detection. Use of process GUID correlates events using Event IDs.
    ![Sysmon event id 1](https://github.com/user-attachments/assets/2da2218f-e8a0-42ee-88d4-5b88c3c7ce14)<br>
    *Ref 11: Event id 1*<br>
    ![Sysmon event id 3](https://github.com/user-attachments/assets/3156fc62-9a5e-4341-8498-bb1daba5a3ee)<br>
    *Ref 12: Event id 3*<br>
    ![Sysmon event id 6-7-8](https://github.com/user-attachments/assets/63f4eb2e-f976-469c-8f55-1212eeddae21)<br>
*Ref 13: Event id 6-7-8*<br>
![Sysmon event id 10](https://github.com/user-attachments/assets/35326e6e-a4be-4a5a-a7af-1a36ac3fd115)<br>
*Ref 14: Event id 10*<br>
![Sysmon event id 22](https://github.com/user-attachments/assets/75b978fd-ecb1-40e4-9dae-efe0b0500437)<br>
*Ref 15: Event id 22*<br>

#Day 9
 1. Installing Sysmon and using Olaf sysmon configuration.xml
    ![Sysmon installed](https://github.com/user-attachments/assets/fbb5aecc-4f10-44e8-80a2-dfa5ca30c36d)<br>
*Ref 16: Sysmon Installed*<br>
![Sysmon event viewer](https://github.com/user-attachments/assets/864a6d45-9853-4142-9ab1-722d392af7b2)<br>
*Ref 17: Sysmon Event Viewer*<br>
![sysmon olaf config](https://github.com/user-attachments/assets/30422d25-63f5-41d7-9a20-2553d91f843d)<br>
*Ref 18: Olaf config*<br>

#Day 10
 1. Ingest Sysmon and Windows Defender logs into Elasticsearch, On the Elastic WebGUI click "Add Integrations. > search for Windows event > Click custom Windows event logs > Click Add custom Winodows event logs > Enter name, description, and for channel name: on your windows server machine find "operational" under Sysmon in event viewer and get "Full Name" from properties > Add integration to abn existing host policy I created > click save and continue, then save and deploy.
 2. Do the same for Windows Defender, but this tiume click advanced options > Include Event IDs in "Event ID" field > add integration the same way.
 3. On Elastic WebGUI go to integrations and custom Windows event logs > Copy "win.logevent_id" Go to discover > paste in fieldname in search and exists for.<br>
 ![sysmon log](https://github.com/user-attachments/assets/1e1a0c3f-fe4d-4467-9f0d-8dbd1714b220)<br>
 *Ref 19: Sysmon log*<br>
![Windows defender log](https://github.com/user-attachments/assets/ba562704-d28a-4d4f-b7d9-384f84bf145e)<br>
*Ref 20: Windows Defender Log*<br>

#Day 11
 1. Investigating Brute Force attacks, Types: 1.Brute Force, 2. Dictionary, 3. Credential Stuffing.
 2. Protect yourself with long complex passwords or use pass phrases, use a password manager, MFA, etc...
 3. Tools used to brute force: 1. Hydra, 2. Hashcat, 3. JohntheRipper<br>
 ![brute force recap](https://github.com/user-attachments/assets/189f0a31-5328-468f-9899-18eeea1fea09)<br>
    *Ref 21: Brute Force Recap*<br>
