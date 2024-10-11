# Caps-30-day-challenge

#Day 1
 1. <a href="https://app.diagrams.net/">Draw.io</a> diagram mapping out the challenge with components, machines, and funtions. Six (6) servers and 2 Host machines with connections, functions, and IP addresses. Connect the Windows and Ubuntu Servers to the Fleet Server. The Fleet Server connects to the Elastic & Kibana Servers for bidirectional communication. Connect the osTicket Server to the ELK Server with a bidirectional connection as well. Connect the Windows and Ubuntu Servers to the ELK Server that connection represents the logs being forwarded from the servers. Map out the Private Network Range. Add in the internet gateway and connect it to a cloud (representation of the internet) and connect the gateway to the VPC. A desktop computer connected to the internet and another computer, the attacker machine w/ Kali Linux (the C2 server being mythic), both connected to the internet.<br>
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

#Day 12
 1. Setup an SSH server using Ubuntu 24.04.
 2. cat auth.log to check for authentication events; use: grep -i failed auth.log - to view only failed login attempts.
 3.  grep -i failed auth.log | grep -i root | cut -d ' ' -f 9 - for all ip address and user name "root"<br>
 ![azure failed login](https://github.com/user-attachments/assets/a40479ef-b286-47dd-9ea0-aa2a105aa9d3)<br>
 *Ref 22: Failed login event*<br>
 
#Day 13
 1. Install elastic agent on SSH Ubuntu server. On elastic webGUI go to fleet > agent policy > create policy, and check what logs are being generated.
 2. Go to view all agent policies > click agents > add agent and install the elastic agent on the Ubunut SSH server.
 3. Back on elastic webGUI go to discover amd view the logs being fed from the Linux agent.name and search for authentication failures.<br>
 ![azure autentication failure](https://github.com/user-attachments/assets/1bb3783a-5250-4840-acb2-951c2e3836ab)<br>
 *Ref 23: Authentication failure*<br>

#Day 14
 1. Create Alerts and Dashboards in Kibana. In elastic webGUI go to discover and filter for your Linux SSH system.
 2. You can filter out agent.name, authentication event, username, source ip, and country name.
 3. Create an alert by clicking Alerts and fill out the query with whatever fits for the situation and then save the rule.
 4. Go to maps and rebuild your search query, click add layer, choose choropleth, fill out the layer and add it to a new dashboard. Save.<br>
 ![azure attempts filter day 14](https://github.com/user-attachments/assets/690aab5e-0a60-42fc-bffb-9af64133f731)<br>
 *Ref 24: Authentication attempts filter*<br>
 ![azure create rule day 14](https://github.com/user-attachments/assets/dac84ccc-29ac-472e-b315-95a8055c0bd7)<br>
*Ref 25: Create a rule*<br>
![azure query day 14](https://github.com/user-attachments/assets/c568d0b1-524c-4e09-a58a-6fd23e03f6e3)<br>
*Ref 26: Query build*<br>
![azure dashboard day 14](https://github.com/user-attachments/assets/05b2cbd8-2c12-4379-a1c3-2ba3317ad486)<br>
*Ref 27: Dashboard*<br>

#Day 15
 1. Discussing RDP where Shadon and Censys are sites where you can find how many RDP ports are being exposed and for what.
 2. To protect yourself: Turn off RDP, turn on MFA, restrict access, use better passwords, and always change your default accounts.<br>
 ![RDP day 15](https://github.com/user-attachments/assets/6e94aee8-b882-4496-a690-0c0848dd9129)<br>
*Ref 28: RDP*

 #Day 16
  1. Alerts and Dashboards part 2. Go to discover on elastic webGUI and sort for your Windows 2022 server.
  2. Filter for code 4625 and review those bruteforce events, add source ip and user.name to the query and save it.
  3. Create an Alert for RDP activities like the SSH activity rule. You can create more robust rules under security, click rules then click detection rules<br>
  ![azure RDP brute force rule day 16](https://github.com/user-attachments/assets/01990f2d-afc2-41f8-916c-8cc874ca9450)<br>
  *Ref 29: RDP brute force rule*<br>
  
#Day 17
 1. Create a dashboard for RDP activity on elastic web GUI in maps section.
 2. Enter your query, add layer, choose choropleth, fill out the layer. Save to existing dashboard.
 3. Create a successful RDP activity map. Make the whole dashboard look better with some visualization tables.<br>
 ![azure alert dashboard day 17](https://github.com/user-attachments/assets/b8769832-6120-4294-baf0-590772c9f8de)<br>
 *Ref 30: Alert dashboard*<br>
![azure final dashboard day 17](https://github.com/user-attachments/assets/ed6e3039-5e31-45d6-9cc0-d55e9bbdf275)<br>
*Ref 31: Final dashboard*<br>

#Day 18
 1. A Command and Control overview, C2 as defined by Miter Att&ck framework.
 2. Attackers need C2 to preform additional actions to get closer to their objective.
 3. Common C2 tools: Metasploit, Cobalt Strike, Sliver, and Mythic which will be used in this challenge.<br>
 ![Mitre framewrok day 18](https://github.com/user-attachments/assets/b413e211-4018-4331-aa11-78406793c419)<br>
 *Ref 31: Mitre Framewrok*<br>
 ![Mythic day 18](https://github.com/user-attachments/assets/7e7c1b89-717e-48c1-952b-79dcef957308)<br>
 *Ref 32: Mythic*<br>

#Day 19
 1. Planning out an "attack" with a diagram; Phase 1, initial access w/ Kali using RDP brute force. Successful Authentication.
 2. Phase 2, execute discovery command: whoami, ipconfig, net user, etc...
 3. Phase 3, In the RDP session disable Windows Defender.
 4. Phase 4, Invoke an expression with PowerShell to download a mythic agent from our C2 server, then execute the mythic agent.
 5. Phase 5, establish command & control.
 6. Phase 6, Download the passwords.txt file.<br>
 ![30-Day-attack-diagram](https://github.com/user-attachments/assets/f719b1d9-10fd-4d0c-89b3-ad3fac3e4205)<br>
 *Ref 33: Attack Diagram*

#Day 20
 1. Set up a Mythic server in your cloud enviroment with Ubuntu 22.04.
 2. Obtain Kali linux while the mythic is being spun up
 3. On your mythic serve install docker-compose, make, and clone the mythic repo.
 4. cd into Mythic directory and install mythic for ubuntu.
 5. run make, restart dicker if failed, run make, start the mythic-cli
 6. log into the mythic webGUI<br>
 ![Mythic day 20](https://github.com/user-attachments/assets/7697c177-2474-4ed4-95ab-0295c4b75665)<br>
 *Ref 34: Mythic*<br>

#Day 21
 1. Setting up a Mythic agent and learning how a brute force attack is carried out.
 2. In windows server create a passwords.txt file with a password in it, change the login password to that password.
 3. Spin up Kali linux machine and open a terminal, cd to wordlists and sudo head -50 rockyou.txt > mydfir-wordlists.txt to copy the 50 passwords from that list into a new file with the addition of your password from before. Create a target.txt and use crowbar to launch a brute force "attack". then use xfreerdp to rdp into the windows server.
 4. Now perform your discovery phase commands.
 5. Disable Windows defender from phase 3.
 6. In Mythic go to mythic-cli anf install the Apollo Agent and C2profile http.
 7. Generate a new payload selecting the target OS > select the C2 profile: changing the call back host to your mythic public ip > Name the payload > create the payload. Right-click download to copy link. In a terminal wget the download, change the payloads name, move the payload into a new directory and star a python3 server with port 9999
 8. In kali linux, in the rdp session open PowerShell and invoke a WebRequest to the mythic machine on port 9999, include -OutFile flag to download the payload. Run the .exe/
 9. In Mythic go to active callbacks, click keyboard icon on the active task to run commands. Use download path and file and exfiltrate passwords.txt<br>
  ![mythic change payload name day 21](https://github.com/user-attachments/assets/822657ba-cb2e-416d-8da3-63060d370695)<br>
 *Ref 35: Change Payload name*<br>
 ![Mythic callback day 21](https://github.com/user-attachments/assets/4b497c6d-2012-41ad-b25b-25f32d079f1e)<br>
*Ref 36: Callback*<br>
 ![Mythic file download day 21](https://github.com/user-attachments/assets/900a9e78-a4ff-439a-a7ba-6a5394c3dff7)<br>
 *Ref 37: passwords.txt download*<br>

#Day 22
 1. Creating more Alerts and Dashboards. Go over to Discover on your elastic webGUI and click New. Search for Events involving the Windows Server and event code 1. Then create a rule to find apollow creation and save it.
 2. Go to detection rules under security and create a new rule, paste in your rule in cutom query, fill out the required fields with timestamp, winlog.event_data.User, Parent Image, ParentCommandLine, and CurretnDirectory > click continue > Name and Describe the rule and save it.
 3. Go to Discover and create another query search for @timestamp, winlog.event_data.User, winlog.event_data.ParentImage, winlog.event_data.ParentCommandLine, winlog.event_data.Image, winlog.event_data.CommandLine, winlog.event_data.CurrentDirectory,winlog.event_data.ProcessGUID,and host.hostname and copy them into your dashboard.
 4. Now go to Dashboards and create a new dashboard then create a visualization with @timestamp, winlog.event_data.User, winlog.event_data.ParentImage, winlog.event_data.ParentCommandLine, winlog.event_data.Image, winlog.event_data.CommandLine, winlog.event_data.CurrentDirectory,winlog.event_data.ProcessGUID,and host.hostname and save and return
 5. Create another visualization for event id 3 with winlog.event_data.Image, winlog.event_data.SourceIp, winlog.event_data.DestinationIp, and winlog.event_data.DestinationPort.
 6. One more for event id 5001 with hostname, ProductName, and event.code<br>
 ![Rule day 22](https://github.com/user-attachments/assets/b102283e-ac15-474d-bba3-86e58eca5e84)<br>
 *Ref 38: Rule*<br>
 ![Required fields day 22](https://github.com/user-attachments/assets/74ed0ba8-b116-43f3-86b7-8b315ad0674d)<br>
 *Ref 39: Required fields*<br>
 ![Mythic rule day 22](https://github.com/user-attachments/assets/22076bbd-56bd-489b-a710-077ea5157eb6)<br>
*Ref 40: Mythic Rule*<br>
![Dashboard day 22](https://github.com/user-attachments/assets/63016fd9-0b31-40c1-864e-d1c070e6f066)<br>
*Ref 41: Dashboard*<br>

#Day 23
 1. Discussing ticketing systems. Tickets can be anything want, containing inofrmation relevant to the situation. To keep track of what is going on and keen audit trail and accountability, saitsfying a part of AAA.
 2. osTicket is an open source ticketing system.<br>
 ![osTicket day 23](https://github.com/user-attachments/assets/c7c3d91b-94b6-4903-b9ef-b122994ed75b)<br>
 *Ref 42: osTicket*<br>

#Day 24
 1. Setting up an <a href="https://osticket.com/"> osTicket</a> system, first deploy a vm witn Windows Server 22022.
 2. Download XAMPP and edit the properties config file; change apache domain name to the vms public ip. start it normally and authorize it to the public, also change the pma. Change php admin config, change bind to the localhost ipv4 address and tcp to the public ip address. Create an endpoint firewall rule to allow inbound connections on ports 80,443, and 3306.
 3. Go back to xampp and start apache and mysql services, then go to apache admin > click phpMyadmin.
 4. Install osTicket, while the 2nd osTicket zip is being extracted navigate to xampp htdocs and create a new osTicket drirectory. Now back at the osTicket download (after extraction) copy/paste scripts and upload folders into the xampp osTicket directory.
 5. In your browser on the osTicket machine enter <the machines public ip>/osticket/upload, change the sample config file inside upload > include, fill out basic installation form and create a new database, install. Now go to phpmyadmin console > new > create DB and assign it to the user on the ip you're using.
 6. Use PowerShell to configure file permission with icacls .\ost-configure.php /reset
 7. Log onto osTicket and go to admin panel.<br>
 ![XAMPP day 24](https://github.com/user-attachments/assets/c274b7b1-0d9c-45f7-81b6-5205a0f96a19)<br>
*Ref 43: XAMPP*<br>
![phpMyAdmin day 24](https://github.com/user-attachments/assets/bdf085c2-6aff-48df-99ac-c566f12a4295)<br>
*Ref 44: phpMyAdmin*<br>
![Installer day 24](https://github.com/user-attachments/assets/5a57a227-5da4-4dbf-b381-9591844fd9e5)<br>
*Ref 45: Installer*<br>
![osticket install complete day 24](https://github.com/user-attachments/assets/b7572b79-e6b1-48b0-b6ff-ef6f4ac4a99c)<br>
*Ref 46: osTicket Install Complete*

#Day 25
 1. Integrating osTIcket and ELK. In osTicket dashboard go to Admin Panel > API> add new API key, enter your private ip (or public ip if the 2 vms are not on the same vpc) address of the ELK Server.
 2. Head over to elastic webGUI and click staj management under management. Then connectors under Alerts and Insights, create connector > choose webhook > <ip address of osticket>/ticket/upload/api/tickets.xml, no auth, add http header: "X-API-Key" with the value of the API key created before > save and test. Create an action (can use the example <a href="https://github.com/osTicket/osTicket/blob/develop/setup/doc/api/tickets.md">HERE</a>). Click run. You can then check your ticketing system on osTicket and if it's there you are full functional.<br>
 ![osticket login day 25](https://github.com/user-attachments/assets/8959674c-51fa-4283-86a5-4cb727b89ae6)<br>
 *Ref 47: osTicket*<br>
 ![so far check list day 25](https://github.com/user-attachments/assets/66703f3e-eab7-4bd7-9a24-2903c5823165)<br>
 *Ref 48: Challenge so Far*<br>

#Day 26
 1. Investigate SSH brute force attacks, select alerts under security in the elastic webGUI, click on timelines, click "all alerts involving a single user" timeline for an example
 2. Go back to alerts and click on an alert (view details); things of note to investigate would be: IP-Is this IP known to perform brute force activity? Any other users affected by this IP? Were any of them successful? What activity occured after successful login? Copy the source ip address and check it in <a href="https://abuseipdb.com/">Abusreipdb.com</a> or <a href="viz.greynoise.io">viz.greynoise.io</a>.  IP-Is this IP known to perform brute force activity? -Yes
 3. In discover paste in the ip address and check user.name to see affected users. -Any other users affected by this IP?
 -yes, 6(root, user, admin, debian, ubuntu, ftp)
 4. Type "and accepted" in the query, realize that it is case sensitive, use "Accepted" with the ip query. -Were any of them successful? -No
 5. Since there were no successes no activity occured. -What activity occured after successful login? -Nothing.
 6. You can now go to Security > actions > webhook: select "For each alert", create a body (use osTicket example taking out attachments and ip), change subject by adding a variable(sule.name, small button on right cornet of box), and message content > save changes. Check your osTicket for generated tickets<br>
 ![Single user timeline day 26](https://github.com/user-attachments/assets/8db91a77-08f7-44a4-a97a-a34b7745a5c0)<br>
*Ref 49: Single user timeline*<br>
![Alert details day 26](https://github.com/user-attachments/assets/e5e3a2e4-ad83-40f1-a3f8-9ecacc48bd51)<br>
*Ref 50: Alert details*<br>
![Asked questions day 26](https://github.com/user-attachments/assets/c79165b8-a102-447b-93a8-0e11305b8f70)<br>
*Ref 51: Investigative questions*
![Abuseipdb day 26](https://github.com/user-attachments/assets/2caccc88-8e98-4d86-af86-a1c09a08ea79)<br>
*Ref 52: Abuseipdb*<br>
![Greynoise day 26](https://github.com/user-attachments/assets/b6b20c85-a625-4da0-bc23-d4e5db7960e7)<br>
*Ref 53: Greynoise*<br>
![affected users day 26](https://github.com/user-attachments/assets/c7b58b72-c3ad-46e0-ab83-874b3129b522)<br>
*Ref 54: Affected users*<br>

#Day 27
 1. Investigate an RDP brute force attack, go to alerts under security in the elastic webGUI, filter for RDP brute force alerts and view details of an alert.
 2. Get the rule to create an osTicket by copying the SSh brute force alert. Copy the body of the actions tab then go to RDP rile and paste it into the body of the webhook action.
 3. We can use the same investigative questioning and process as the SSH brute force attack: Brute force -  106.75.134.63 IP-Is this IP known to perform brute force activity? -yes Any other users affected by this IP? -yes, 6(root, user, admin, debian, ubuntu, ftp), Were any of them successful?  -No, -What activity occured after successful login? -Nothing. Carry out the investigation in the same way using the same steps using event.code: 4624 instead of "Accepted"
 4. If there were a successful brute force; Brute force - 173.171.129.183, IP-Is this IP known to perform brute force activity? -yes, Any other users affected by this IP? -No, Were any of them successful? -Yes, What activity occured after successful login? -Sep 24, 2024 @ 01:10:43.011, What happened here!?  -potential end time: - Sep 24, 2024 @ 03:59:36.828. Filter out by "<source.ip> and event.code: 4624 and <user.name> and <winlog.event_data.TargetLogonId>" or "<user.name> and <winlog.event_data.TargetLogonId" > follow the chain of events. If you follow the brute force attempt we did a few days ago, you can see all the steps completed with our successful logon, like assigning special priveleges. If you analyze timestamps you may notice if it's an automated(brute force) session.<br>
 ![alerts day 27](https://github.com/user-attachments/assets/52f4c6c0-61d3-44c4-8fa8-7db62ef5b77d)<br>
 *Ref 55: Alerts*<br>
 ![osticket rdp bruteforce day 27](https://github.com/user-attachments/assets/aa6c54f8-f9d5-4a13-84e1-9015d9e1ea5b)<br>
*Ref 56: osTicket rdp*<br>
![abuseipdb rdp day 27](https://github.com/user-attachments/assets/3a1c63be-03e5-4bbd-aed4-466f327101e1)<br>
*Ref 57: Abuseipdb.com rdp*
![greynoise rdp day 27](https://github.com/user-attachments/assets/645121f4-7c68-481a-b539-546fafac6c14)<br>
*Ref 58: greynoise.io rdp*<br>
![Successful RDP day 27](https://github.com/user-attachments/assets/b00063f8-da0e-43b5-9738-80667b3b9196)<br>
*Ref 59: Successful RDP questions*<br>
![special priveleges assigned day 27](https://github.com/user-attachments/assets/ff08bdfd-7a4e-478a-958b-7243f5eca701)<br>
*Ref 60: Special Priveleges Assigned*<br>
![event code and 4624 day 27](https://github.com/user-attachments/assets/3dff41f1-9f5d-4235-9618-c696d8652496)<br?
*Ref 61: event.code 4624*<br>

#Day 28
 1. Investigate the Mythic agent in elastic, on the discover screen type in your svchost.exe, sort old to new and follow the chain of events. You can check for a lot of back and forth traffic indicative to a C2 session, or look at a "heartbeat" and use <a href="https://www.blackhillsinfosec.com/projects/rita/">RITA</a>, or look at process and network creations.
 2. Go to your dashboards and select suspicious activity, look out for strange processes and/or dashboards. Use the winlog.event_data.DestinationIp of a suspicious event and filter it out with event.code: 3 on the discover screen and build a timeline.
 3. in one of the events look for the processGUID and copy it. paste it into your query to filter out discover, investigate an event to see what the user did: processes opened?, From where?, When?, What did they do? Pay attention to even codes. find file hashes for files you are investigationg. Search for the processGUID of the C2 agent you used previously, find the processId and ParenProcessId yo filter out events. Also search for the passwords.txt file and examine those events.
 4. Go to rules under Security > click on detection rules > edit rule settings > Actions and copy the body > paste it into the body of the mythic agent rule (create the webhook if needed). You can edit rule settings to include fields that are relevant to you under edit rule settings > about and find the "custom higlighted fields"
 5. In osTicket click on the newly generated ticket, get spme information thats important for your investigation and make a query in elastic discover. Run your investigation and create a timeline and realize elastic recorded everything that was done.<br>
 ![suspicious ip day 28](https://github.com/user-attachments/assets/b04cdcf3-d203-487f-b2a6-314e5ae28dc9)<br>
 *Ref 62: Suspicious IP*<br>
![event code 3 and destination ip filter day 28](https://github.com/user-attachments/assets/7fb0ca71-0c07-49dc-a248-da1eef5a67c7)<br>
*Ref 63: Event code 3 and destination ip filter*<br>
![Process GUID day 28](https://github.com/user-attachments/assets/74bceca5-df5b-4328-8bb5-947c33ec0080)<br>
*Ref 64: ProcessGUID*<br>
![svchost creation amd sha day 28](https://github.com/user-attachments/assets/31ca2d07-66cd-4eb8-ba11-d2ccef1240ab)<br>
*Ref 65: svchost creation and SHA*<br>
![investigation notes day 28](https://github.com/user-attachments/assets/b20721fc-d05c-42e9-b489-7b0cfd88fe27)<br>
*Ref 66: Investigation timeline and notes*<br>

#Day 29
 1. Set up Elastic Defend, Elastics EDR. In elastic click into integrations > Elastic Defend > Add Elastic Defend, name it, select traditional Endpoints and complete EDr oprion, add to existing host and choose the Windows Sever policy > save and continue > save and deploy.
 2. Go to manage under Security > click endpoint >actions (all the way to the right)
 3. In the discover section enter malware into a new query, and look for a malware prevention alert. You can also check for an alert under Security > Alerts, to open the alert and assess it.
 4. To do a response action click on the alert > edit rule settings > Elastic Defend > isolate >save change.
 5. Investigate the malware you just tested.<br>
 ![EDR namin day 29](https://github.com/user-attachments/assets/947d5141-eefa-40ca-b4a1-c5c62a994578)<br>
*Ref 67: EDR naming*<br>
 ![EDR integrated day 29](https://github.com/user-attachments/assets/dedd47be-f615-457b-a583-393c2c2016d3)
*Ref 68: EDR integrated*<br>
 ![Elastic endpoints day 29](https://github.com/user-attachments/assets/cfefbd83-0334-4815-add2-f35683090ae4)<br>
*Ref 69: Elastic endpoints*<br>
![Malware query day 29](https://github.com/user-attachments/assets/f810f7a6-517f-42a8-ab30-7c4e2c483fc8)<br>
*Ref 70: Malware query*
![Malware Alert day 29](https://github.com/user-attachments/assets/eaba7fec-b5f3-4e60-a1f0-bd1aa6675b32)<br>
*Ref 71: Malware Alert*<br>
![respone action day 29](https://github.com/user-attachments/assets/25a5f51e-7b49-4c9c-929a-b51b5f4473ce)<br>
*Ref 72: Response action*<br>
![Elastic security day 29](https://github.com/user-attachments/assets/3b5aa191-e110-4a01-b434-6c6cc9606e88)<br>
*Ref 73: Elastic Security*<br>

#Day 30
 1. This was a day about troubleshooting and clean up. Gather thoughts and reflecting on the challenge. It was frustrating at times, but once I was able to troubleshoot the problems I ran into, that feeling made everything worth it. I do have a passion for this stuff and I just want to keep moving forward with it. The challenge felt great and was worth the setbacks and even long nights getting things to work. Thanks for everything Steven!
