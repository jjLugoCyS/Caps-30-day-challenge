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
