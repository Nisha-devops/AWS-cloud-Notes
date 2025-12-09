🛠️ AWS VPC & VPC Peering – Troubleshooting Guide (Q&A)

Welcome to my AWS VPC & VPC Peering Troubleshooting Repository!
This collection contains real-world issues, interview-focused questions, and practical troubleshooting steps that Cloud & DevOps engineers use while diagnosing networking problems in AWS.

This guide is designed to help:

AWS Beginners

Cloud Support/DevOps Engineers

Interview Preparation

Anyone learning AWS Networking


📌 1. What is the first thing to check when an EC2 instance cannot connect to the internet?
✅ Answer

Check if the instance is in a public subnet with:

Public IP assigned

Subnet route table has a route to the Internet Gateway (0.0.0.0/0 → IGW)

📝 Explanation

If IGW or public IP is missing, outbound traffic cannot go to the internet.


📌 2. A Private EC2 instance cannot access the internet. What to check?
✅ Answer

Verify:

Private subnet route table → 0.0.0.0/0 → NAT Gateway

NAT Gateway is in a public subnet

NATGW has an Elastic IP

No NACL blocking outbound 1024–65535

📝 Explanation

Private instances use NAT to access the internet.
Incorrect routing or misconfigured NAT causes failures.


📌 3. Two instances in the same VPC cannot communicate. Why?
🔍 Checklist

Security Groups allow inbound rules

NACLs allow both inbound & outbound

Instances are in overlapping CIDR ranges?

📝 Explanation

Even in same VPC, a restrictive SG/NACL will block the traffic.


📌 4. VPC Peering is active but traffic is not flowing between VPCs. What to check?
⚠️ Most Common Reasons

Routes missing

VPC-A route table → Add CIDR of VPC-B → peering connection

VPC-B route table → Add CIDR of VPC-A → peering connection

Overlapping CIDR ranges

VPC Peering does NOT work if CIDRs overlap.

Security Groups not allowing incoming traffic

Add inbound rule allowing source = other VPC’s CIDR

NACL blocking

Check both inbound and outbound rules


📌 5. Does VPC Peering support transitive routing?
❌ Answer: No

If A ↔ B and B ↔ C are peered:
A cannot talk to C.

📝 Explanation

Peering is point-to-point only.
Use Transit Gateway for transitive communication.


📌 6. Why can’t two EC2s ping each other across VPC Peering?
❗ Reasons:

ICMP is blocked by default in SG

Add inbound rule:
SG → Allow ICMP Echo Request (Type 8)

📝 Explanation

Ping requires ICMP, not TCP/UDP.


📌 7. VPC Peering is pending for long time — why?
Common causes:

Requester accepted but Accepter not approved

Wrong account selected

Wrong region

Fix:

Go to VPC → Peering Connections → Accept Request.


📌 8. NAT Gateway created but instance still has no internet.
Checklist:

NAT in Public Subnet?

Public subnet has route to IGW?

Route table of private subnet pointing to NAT?

NACL blocking return traffic?


📌 9. Cannot SSH into EC2. Steps to check

Public IP assigned?

Correct Key Pair used?

SG inbound rule → port 22 open?

NACL inbound/outbound open?

Routing correct?

Linux instance username correct (ec2-user/ubuntu)?


📌 10. How to check if CIDR blocks overlap?

Use a CIDR overlap calculator OR check manually:

Example:

VPC-A → 10.0.0.0/16

VPC-B → 10.0.0.0/24

These overlap → Peering will not work.



📘 Included Topics

VPC Troubleshooting (EC2, Subnets, Routing)

VPC Peering Troubleshooting

Routing Table Issues

CIDR Problems

SG/NACL Debugging

NAT Gateway Problems

Interview-Focused Q&A

🎯 Purpose of This Repository

This project demonstrates my real-time understanding of AWS networking by solving practical issues.

🚀 Author

Nisha – Cloud & DevOps Enthusiast
Sharing notes, troubleshooting, hands-on labs, and cloud learning path through GitHub.
