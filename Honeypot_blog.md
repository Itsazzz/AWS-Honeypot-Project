Building a Comprehensive Honeypot on AWS: A Deep Dive

Hands-on experience is crucial in cybersecurity, and creating a honeypot is one of the best ways to learn about attacker behavior and defense mechanisms. In this project, I deployed a honeypot on AWS to simulate real-world attacks and observe how attackers interact with the system. This detailed blog will walk you through each configuration and insight gained.

What is a Honeypot?

A honeypot is a decoy system designed to attract attackers, log their activities, and analyze their methods. Unlike real systems, honeypots are intentionally vulnerable, allowing researchers and cybersecurity professionals to learn about attack patterns and improve defenses without compromising actual production environments.

Why Use AWS?

AWS (Amazon Web Services) provides an ideal platform for deploying honeypots due to its scalability, accessibility, and robust security features. Here's why AWS stands out:

Scalability: Easily adjust resources to suit project needs.

Accessibility: Public IPs allow real-time attack simulations.

Security: Flexible security groups enable precise traffic control.

Cost-Effectiveness: The free-tier allows beginners to experiment without incurring costs.

Project Goals

The main objectives of this project were:

Deploy a honeypot on AWS to attract attackers.

Capture and analyze logs to understand attack methods.

Simulate an attack-defense scenario with designated roles:

Person 1: Configured and managed the honeypot.

Person 2: Played the role of the attacker.

Person 3: Acted as the defender, monitoring and mitigating threats remotely.

Step-by-Step Deployment

1. Launching an AWS EC2 Instance

AWS EC2 (Elastic Compute Cloud) allows you to create virtual machines. Here's how I set up the instance:

Log in: Access the .

Create Instance:

Selected Ubuntu Server 22.04 LTS as the operating system.

Chose t2.micro as the instance type (free-tier eligible).

Security Group Configuration:

Allowed inbound traffic on:

Port 2222 for the honeypot.

Port 22 for administrative SSH (restricted to my IP).

Key Pair: Generated a key pair for secure access and downloaded the .pem file.

Launch: Started the instance and noted its public IPv4 address.

2. Setting Up Cowrie Honeypot

Cowrie is a popular SSH/Telnet honeypot that logs all interactions. Here’s the setup process:

Step 1: Connect to the Instance

SSH into the instance using the key pair:

ssh -i <key-pair-name>.pem ubuntu@<public-ip>

Step 2: Update System and Install Dependencies

Ensure the system is up-to-date:

sudo apt update && sudo apt upgrade -y

sudo apt install git python3-venv python3-pip -y

Step 3: Clone and Configure Cowrie

Clone the repository:

git clone https://github.com/cowrie/cowrie.git

cd cowrie

Set up a virtual environment:

python3 -m venv cowrie-env

source cowrie-env/bin/activate

Install dependencies:

pip install -U pip setuptools

pip install -r requirements.txt

Configure Cowrie:

cp etc/cowrie.cfg.dist etc/cowrie.cfg

nano etc/cowrie.cfg

Set the listening port to 2222:

[ssh]

listen_port = 2222

Step 4: Start Cowrie

Start the honeypot:

./bin/cowrie start

Verify its status:

./bin/cowrie status

Step 5: Test the Setup

Attempt to connect to the honeypot:

ssh -p 2222 anyuser@<public-ip>

This interaction will be logged by Cowrie.

Simulating an Attack

Role of the Attacker

Tools Used:

Hydra: For brute-force password attempts.

Metasploit Framework: To exploit vulnerabilities.

Steps:

Identified the honeypot’s IP and port (2222)

Used Hydra for brute-forcing:

hydra -l root -P passwords.txt ssh://<public-ip>:2222

Exploited vulnerabilities with Metasploit:

msfconsole

use exploit/unix/ssh/sshexec

set RHOST <public-ip>

set RPORT 2222

run

Observed and documented the honeypot’s responses.

Role of the Defender

Tools Used:

Honeypot logs located at /home/cowrie/cowrie/var/log/cowrie.log.

AWS Security Groups for blocking malicious IPs.

Steps:

Monitored logs in real time:

tail -f /home/cowrie/cowrie/var/log/cowrie.log

Identified suspicious IPs from the logs.

Blocked malicious IPs using AWS CLI:

aws ec2 revoke-security-group-ingress --group-id <group-id> --protocol tcp --port 2222 --cidr <malicious-ip>/32

Documented attack patterns for analysis.

Results and Key Insights

Honeypot Effectiveness

Captured all attack attempts, including commands, IPs, and timestamps.

Successfully mimicked a vulnerable SSH service to attract attackers.

Defensive Success

Real-time monitoring enabled immediate detection and mitigation of threats.

Blocking malicious IPs reduced repeated attempts.

Lessons Learned

Logging provides critical insights into attacker behavior.

Honeypots are invaluable for testing real-world defense strategies.

Collaboration between attackers and defenders enhances understanding of both offensive and defensive tactics.

Conclusion

This project demonstrated the power of honeypots in cybersecurity. Deploying Cowrie on AWS allowed me to study attack patterns and test defense mechanisms in a controlled environment. The experience reinforced the importance of proactive monitoring and logging.