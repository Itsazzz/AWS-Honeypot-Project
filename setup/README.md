# Honeypot Setup Guide

This guide details the process of setting up a honeypot on AWS using the Cowrie SSH/Telnet honeypot.

## Step 1: Launch an AWS EC2 Instance
  1. **Log in**: Access the [AWS Management Console](https://aws.amazon.com/console/).
  2. **Create Instance**:
     - Select **Ubuntu Server 22.04 LTS**.
     - Choose the **t2.micro** instance type (free-tier eligible).
     - Configure the security group:
     - Allow inbound traffic on:
     - **Port 2222** (honeypot).
     - **Port 22** (administrative SSH, restricted to your IP).
     - Generate a **key pair** and download the `.pem` file.
  3. **Launch**: Start the instance and note its public IPv4 address.

## Step 2: Install and Configure Cowrie
  1. **Connect to the instance**:
     ```bash
     ssh -i <key-pair-name>.pem ubuntu@<public-ip>
  2. **Update the system**:
     ```bash
     sudo apt update && sudo apt upgrade -y
     sudo apt install git python3-venv python3-pip -y
  3. **Clone the Cowrie repository**
     ```bash
     git clone https://github.com/cowrie/cowrie.git
     cd cowrie
  4. **Set up a virtual environment**:
     ```bash
     python3 -m venv cowrie-env
     source cowrie-env/bin/activate
  5. **Install Dependencies**:
      ```bash
      pip install -U pip setuptools
      pip install -r requirements.txt
  6. **Conffigure Cowrie**:
       ```bash
       cp etc/cowrie.cfg.dist etc/cowrie.cfg
       nano etc/cowrie.cfg
       **Set the listening port to 2222**
        [ssh]
       listen_port = 2222
 7. **Start Cowrie**:
     ```bash
     ./bin/cowrie start
     ./bin/cowrie status






