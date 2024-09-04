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
