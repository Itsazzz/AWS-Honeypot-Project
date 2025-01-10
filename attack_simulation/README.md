# Attack Simulation

This section covers how to simulate attacks on the honeypot to analyze its behavior.

## Role of the Attacker
### Tools Used:
- **Hydra**: For brute-force password attempts.
- **Metasploit Framework**: To exploit vulnerabilities.

### Steps:
1. Identify the honeypot’s IP and port (**2222**).

2. **Use Hydra for brute-forcing**:
   ```bash
    hydra -l root -P passwords.txt ssh://<public-ip>:2222

3. **Exploit vulnerabilities with Metasploit**:
   ```bash
    msfconsole
    use exploit/unix/ssh/sshexec
    set RHOST <public-ip>
    set RPORT 2222
    run


##Role of the Defender
##Tools Used:
- **Honeypot logs**: /home/cowrie/cowrie/var/log/cowrie.log.
-**AWS Security Groups for blocking malicious IPs**.


##Steps:
1. Monitor logs in real time:
  ```bash
  tail -f /home/cowrie/cowrie/var/log/cowrie.log

2. **Identify suspicious IPs and block them**:
  ```bash
  aws ec2 revoke-security-group-ingress --group-id <group-id> --protocol tcp --port 2222 --cidr <malicious-ip>/32
