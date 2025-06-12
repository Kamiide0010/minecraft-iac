# Minecraft Infrastructure as Code (IaC)

This project automates the configuration, and deployment of a Minecraft server using Infrastructure as Code (IaC) principles. The setup is compliant with course rules and avoids the AWS Management Console.

## Requirements
- Terraform >= 1.0
- AWS CLI v2
- Minecraft Server `.jar`
- AWS Academy Learner Lab temporary credentials

## Project Overview
This project provisions:
- An EC2 instance (Amazon Linux 2, t2.micro)
- A security group allowing:
  - SSH from your IP (port 22)
  - Minecraft traffic from 0.0.0.0/0 (port 25565)
- Installs and runs a Minecraft server

## Setup Instructions

### 1. Configure AWS Credentials

Use AWS Academy Learner Lab credentials and paste into:

```
notepad %USERPROFILE%\.aws\credentials
```

```
[default]
aws_access_key_id = YOUR_ACCESS_KEY
aws_secret_access_key = YOUR_SECRET_KEY
aws_session_token = YOUR_SESSION_TOKEN
```

### 2. Run Terraform

Navigate to the `terraform` folder and run:

```bash
terraform init
terraform apply -auto-approve
```

### 3. Connect to EC2

```bash
ssh -i Minecraft-Server.pem ec2-user@<YOUR_EC2_PUBLIC_IP>
```

### 4. Install Minecraft

```bash
sudo yum update -y
sudo amazon-linux-extras enable corretto21
sudo yum install java-21-amazon-corretto -y
wget https://piston-data.mojang.com/v1/objects/e6ec2f64e6080b9b5d9b471b291c33cc7f509733/server.jar -O server.jar
echo "eula=true" > eula.txt
java -Xmx1024M -Xms1024M -jar server.jar nogui
```

## Communication Contract

| Port | Protocol | Source        | Purpose            |
|------|----------|---------------|--------------------|
| 22   | TCP      | YOUR_IP/32    | SSH Access         |
| 25565| TCP      | 0.0.0.0/0     | Minecraft Access   |
| All  | All      | 0.0.0.0/0     | Default Outbound   |

## Sequence Diagram

Refer to `diagrams/sequence-diagram.png`.

## Mitigation Plan

- **Credential Expiry**: Regenerate from AWS Academy Lab.
- **IP Change**: Update Terraform CIDR block and re-apply.
- **Port Blocked**: Use an open network or VPN.

## Resources

- Terraform AWS Provider Docs
- Minecraft Server Download
