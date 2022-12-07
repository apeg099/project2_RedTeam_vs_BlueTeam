# RedTeam vs BlueTeam

## Red Team

### Monitoring Setup Instructions


As the you attack a web server today, it will send all of the attack info to an ELK server.


The following setup commands need to be run on the Capstone machine before the attack takes place in order to make sure the server is collecting logs.


Be sure to complete these steps before starting the attack instructions.


### Instructions


1. Open the 'HyperV Manager'.


2. Choose the Capstone machine from the list of Virtual Machines and open a terminal window.


3. Login to the machine using the credentials: vagrant:tnargav


4. Switch to the root user with sudo su



#### Setup Filebeat
Run the following commands:

filebeat modules enable apache
filebeat setup

#### Setup Metricbeat
Run the following commands:

metricbeat modules enable apache
metricbeat setup

#### Setup Packetbeat
Run the following command:

packetbeat setup

Restart all 3 services. Run the following commands:

systemctl restart filebeat
systemctl restart metricbeat
systemctl restart packetbeat

These restart commands should not give any output:

Once all three of these have been enabled, close the terminal window for this machine and proceed with your attack.


#### Attack!
Act as an offensive security Red Team to exploit a vulnerable Capstone VM.
You will need to use the following tools, in no particular order:

- Firefox
- Hydra
- Nmap
- John the Ripper
- Metasploit
- curl
- MSVenom


Setup
Your entire attack take place using the Kali Linux Machine.


Inside the HyperV Manager, double-click on the Kali machine to bring up the VM login window.


Login with the credentials: root:toor

# Incident Analysis with Kibana

## Instructions
Analyzing a log will give more in-depth details about the incident. It will teach you:


- What your attack looks like from a defender's perspective.


- How stealthy or detectable your tactics are.


- Which kinds of alarms and alerts SOC and IR professionals can set to spot attacks like yours while they occur, rather than after.



### Adding Kibana Log Data
1. You will need to import filebeat, metricbeat and packetbeat data.
2. Open Google Chrome icon on the Windows host's desktop to launch Kibana. If it doesn't load as the default page, navigate to http://192.168.1.105:5601.
3. This will open 4 tabs automatically.
4. Click on the Explore My Own link to get started.

#### Adding Appache logs
1. Click on Add Log Data

2. Click on Apache logs

3. Scroll to the bottom of the page and click on 'Check Data'
You should see a message highlighted in green: Data successfully received from this module

4. Return to the Home screen by moving back 2 pages.

#### Adding System Logs
1. Click on Add Log Data
2. Click on System logs

3. Scroll to the bottom of the page and click on 'Check Data'
You should see a message highlighted in green: Data successfully received from this module

4. Return to the Home screen by moving back 2 pages.

#### Adding Apache Metrics
1. Click on Add Metric Data

2. Click on Apache Metrics

3. Scroll to the bottom of the page and click on 'Check Data'
You should see a message highlighted in green: Data successfully received from this module

4. Return to the Home screen by moving back 2 pages.

#### Adding System Metrics
1. Click on Add Metric Data

2. Click on System Metrics

3. Scroll to the bottom of the page and click on 'Check Data'
You should see a message highlighted in green: Data successfully received from this module

4. Close the browser and re-open it.

### Dashboard Creation
Create a Kibana dashboard using the pre-built visualizations. On the left navigation panel, click on Dashboards.
Click on Create dashboard in the upper right hand side.

#### On the new page click on Add an existing to add the following existing reports:

- HTTP status codes for the top queries [Packetbeat] ECS
- Top 10 HTTP requests [Packetbeat] ECS
- Network Traffic Between Hosts [Packetbeat Flows] ECS
- Top Hosts Creating Traffic [Packetbeat Flows] ECS
- Connections over time [Packetbeat Flows] ECS
- HTTP error codes [Packetbeat] ECS
- Errors vs successful transactions [Packetbeat] ECS
- HTTP Transactions [Packetbeat] ECS

#### On the Discover page, locate the search field.
Start typing source and notice the suggestions that come up.
Search for the source.ip of the attacking machine.
Use 'AND' and 'NOT' to further filter you search and look for communications between your attacking machine and the victim machine.
Other things to look for:

- url
- status_code
- error_code

### Identify the offensive traffic.

Identify the traffic between your machine and the web machine:

- When did the interaction occur?
- What responses did the victim send back?
- What data is concerning from the Blue Team perspective?





### Find the request for the hidden directory.

In your attack, you found a secret folder. Let's look at that interaction between these two machines.

- How many requests were made to this directory? At what time and from which IP address(es)?
- Which files were requested? What information did they contain?
- What kind of alarm would you set to detect this behavior in the future?
- Identify at least one way to harden the vulnerable machine that would mitigate this attack.





### Identify the brute force attack.

After identifying the hidden directory, you used Hydra to brute-force the target server. Answer the following questions:

- Can you identify packets specifically from Hydra?
- How many requests were made in the brute-force attack?
- How many requests had the attacker made before discovering the correct password in this one?
- What kind of alarm would you set to detect this behavior in the future and at what threshold(s)?
- Identify at least one way to harden the vulnerable machine that would mitigate this attack.





### Find the WebDav connection.

Use your dashboard to answer the following questions:

- How many requests were made to this directory?
- Which file(s) were requested?
- What kind of alarm would you set to detect such access in the future?
- Identify at least one way to harden the vulnerable machine that would mitigate this attack.





### Identify the reverse shell and meterpreter traffic.

To finish off the attack, you uploaded a PHP reverse shell and started a meterpreter shell session. Answer the following questions:

- Can you identify traffic from the meterpreter session?
- What kinds of alarms would you set to detect this behavior in the future?
- Identify at least one way to harden the vulnerable machine that would mitigate this attack.
