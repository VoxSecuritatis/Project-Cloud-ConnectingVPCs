# Project: Connecting VPCs
Date:  2025-02-22

## Description
#### Allow communication between applications hosted in different VPCs by using VPC peering. The Marketing and Developer EC2 instances need to access the Financial Services server in the Finance department's VPC.

## Tools Used
- AWS Console
- AWS EC2
- AWS VPC

## Procedure
#### Step 1
#### In this solution, separate virtual private clouds (VPCs) have been created for the Marketing, Finance, and Development departments. The Finance VPC contains resources that the Marketing and Developer VPCs need to access.
<img src="https://imgur.com/oyvCl4Q.jpg" height="80%" width="80%" alt="Architecture Diagram"/>

---
#### Concept:  Amazon Virtual Private Cloud (Amazon VPC) is a service that helps you launch AWS resources in a logically isolated virtual network that you define. You have complete control over your virtual networking environment.
Step 2
1. Open the AWS Console
2. In the top navigation bar search box, type: vpc 
3. In the search results, under Services, click VPC.
4. Go to the next step.
<img src="https://imgur.com/GhYfEOt.jpg" height="80%" width="80%" alt="Open AWS Console"/>
<img src="https://imgur.com/84LYN1b.jpg" height="80%" width="80%" alt="Launch VPC service"/>

---
#### Concept:  By default, VPCs are isolated from each other. A VPC peering connection is a networking connection between two VPCs that you can use to route traffic between them by using private IP addresses. 
Step 3
1. In the left navigation pane, click Your VPCs. 
2. In the Your VPCs section, review the available Marketing, Finance, and Developer VPCs. 
3. Go to the next step.
<img src="https://imgur.com/nviEtum.jpg" height="80%" width="80%" alt="View VPCs"/>

---
Step 4
1. In the top navigation bar search box, type: ec2 
2. In the search results, under Services, click EC2. 
3. Go to the next step.
<img src="https://imgur.com/Bn4a01A.jpg" height="80%" width="80%" alt="Launch EC2 service"/>

---
Step 5
1. On the EC2 Dashboard, in the Resources section, click Instances (running). 
2. Go to the next step.
<img src="https://imgur.com/SzkQMTI.jpg" height="80%" width="80%" alt="EC2 instances running"/>

---
#### Concept: A subnet represents a range of IP addresses within a VPC. Each subnet must be contained entirely within a single Availability Zone (AZ) and cannot span across multiple AZs. 
Step 6
1. In the Instances section, choose the check box to select the FinanceServer instance. 
2. Below that section, click the Networking tab. 
3. Review to see that no Public IPv4 address or Public IPv4DNS is populated. - An instance created in a private subnet has no public addresses. 
4. Under Private IPv4 addresses, click the copy icon to copy the provided IP address, and then paste it in the text editor of your choice on your device. - You use this address in later steps. 
5. Under Subnet ID, review the provided ID. 
6. Go to the next step.
<img src="https://imgur.com/Lu233Pd.jpg" height="80%" width="80%" alt="EC2 instance selection"/>

---
#### Concept: You can connect to an Amazon Elastic Compute Cloud (Amazon EC2) instance in four ways: - EC2 Instance Connect - Session Manager - SSH client - EC2 Serial Console 
Step 7
1. In the Instances section, clear the check box to deselect the FinanceServer instance. 
2. Choose the MarketingServer instance. 
3. Below that section, on the Details tab, under VPC ID, review the VPC that the MarketingServer instance belongs to. 
4. At the top of the Instances section, click Connect. 
5. Go to the next step.
<img src="https://imgur.com/7gVkkcq.jpg" height="80%" width="80%" alt="EC2 connect to Marketing VPC"/>

---
#### Concept: Session Manager is a fully managed AWS Systems Manager capability. With Session Manager, you can manage your Amazon EC2 instances, edge devices, and on-premises servers and virtual machines (VMs). You can use either an interactive one-click browser-based shell or the AWS Command Line Interface (AWS CLI). 
Step 8
1. Click the Session Manager tab. 
2. Click Connect. - The Session Manager terminal for the MarketingServer instance opens in a new browser tab (or window). 
3. Go to the next step.
<img src="https://imgur.com/JRKTXb3.jpg" height="80%" width="80%" alt="EC2 connect to instance"/>

---
#### Concept:  By default, VPCs cannot communicate with resources in other VPCs by using private IPv4 addresses or IPv6 addresses.
Step 9
1. In the Session Manager terminal window, replacing the current IP address with the IP address of the FinanceServer that you copied in an earlier step, run (type the command and press Enter): ping 172.31.x.xx - This command checks the connection to the FinanceServer instance. 
2. Review to see that your command hangs, and there is no connection. There is no error alert. - Because the FinanceServer instance uses only a private IP address, VPCs cannot establish routing paths to destinations outside their local private IP range. 
3. To exit the running process, on your keyboard, press Ctrl+C. 
4. Go to next the step.
<img src="https://imgur.com/wgftQDK.jpg" height="80%" width="80%" alt="Ping Finance from Marketing VPC"/>

---
#### Concept: EC2 instances reside within a subnet in a VPC.
Step 10
1. Return to the Amazon EC2 console browser tab. 
2. Review to confirm that the MarketingServer instance is still selected. 
3. On the Networking tab, under Subnet ID, click the ID for MarketingPublicSubnet1. 
4. Go to the next step.
<img src="https://imgur.com/bI5cGEN.jpg" height="80%" width="80%" alt="Return to EC2 console"/>

---
#### Concept: An important property of a subnet is its route table. A route table is a collection of routing rules that manages traffic from any associated subnet. It contains specific routes that determine where network traffic should be directed from your subnet or gateway.
Step 11
1. In the Subnets section, choose the available subnet name. 
2. On the Details tab, under Route table, click the provided route ID. - The VPC dashboard opens in a new browser tab (or window). 
3. Go to the next step.
<img src="https://imgur.com/MfG3k7J.jpg" height="80%" width="80%" alt="Routing table - Marketing VPC"/>

---
#### Concept: If an internet gateway is attached, route tables have rules for local traffic and public IP ranges. We recommend that you specify a CIDR block from the private IPv4 address ranges, as specified in RFC 1918. 
Step 12
1. In the Route tables section, choose the available Marketing route table. 
2. On the Routes tab, review the routing rules. - Two routes should be displayed: one route for local traffic and one route for internet traffic through the internet gateway.
3. Go to the next step.
<img src="https://imgur.com/ZlIGiEV.jpg" height="80%" width="80%" alt="Reviewing Marketing VPC route table"/>

---
#### Concept: With a VPC peering connection, instances in either VPC can communicate with each other as if they are in the same network.
Step 13
1. In the left navigation pane, click Peering connections. 
2. In the Peering connections section, click Create peering connection. 
3. Go to the next step.
<img src="https://imgur.com/OnqO9Wb.jpg" height="80%" width="80%" alt="Create peering connection"/>

---
#### Concept:  Your VPC will request that another VPC allow access to its resources. The VPC that makes the request is called the Requester. You can request access to VPCs from other AWS accounts. 
Step 14
1. In the Peering connection settings section, for Name, type: Marketing <> Finance 
2. For VPC ID (Requester), on the dropdown list, choose the Marketing VPC. 
3. For VPC CIDRs, review to confirm that the Marketing VPC has 10.10.0.0/16 as its CIDR range. 
4. Go to the next step.
<img src="https://imgur.com/bTvXS49.jpg" height="80%" width="80%" alt="Peering connection settings - part 1"/>


---
#### Concept:  VPCs in a peering connection cannot have overlapping CIDR blocks. For example, if two VPCs both have the CIDR block 192.168.0.0/28, a peering connection cannot be created between them.
Step 15
1. For VPC ID (Accepter), choose the Finance VPC. 
2. For VPC CIDRs, review to confirm that the Finance VPC has 172.31.0.0/16 as its CIDR range. 
3. Click Create peering connection. 
4. Go to the next step.
<img src="https://imgur.com/tAop7gJ.jpg" height="80%" width="80%" alt="Peering connection settings - part 2"/>

---
#### Concept: To request a VPC peering connection with a VPC in your account, make sure that you have the IDs of the VPCs with which you are creating the VPC peering connection. You must both create and accept the VPC peering connection request yourself to activate it. If the peering connection is across accounts, both accounts must accept the connection to activate it.
Step 16
1. In the success alert, review the message. 
2. Under Status, review to confirm that the status is Pending Acceptance. 
3. Click Actions to expand the dropdown list. 
4. Choose Accept request. 
5. Go to the next step.
<img src="https://imgur.com/GIjxPAl.jpg" height="80%" width="80%" alt="VPC peering request Marketing VPC to Finance VPC"/>

---
Step 17
1. In the pop-up box, click Accept request. 
2. Go to the next step.
<img src="https://imgur.com/uIHsevP.jpg" height="80%" width="80%" alt="Accept VPC peering connection pop-up"/>

---
Step 18
1. In the success alert, review the message. 
2. Under Status, review to see that the status is Active. 
3. Go to the next step.
<img src="https://imgur.com/KvkJkQm.jpg" height="80%" width="80%" alt="Peering success alert"/>

---
#### Concept: After you establish a peering connection, you must modify the route table associated with each VPC. You must add a route into each route table to allow traffic to be routed to the peered VPC.
Step 19
1. Return to the Instances section on the Amazon EC2 console in the other browser tab. 
2. Choose the MarketingServer instance. 
3. On the Details tab, scroll down to Subnet ID. 
4. Click the provided ID. 
5. Go to the next step.
<img src="https://imgur.com/4e4BWcG.jpg" height="80%" width="80%" alt="Add route from Marketing VPC to Finance VPC - part 1"/>

---
Step 20
1. In the Subnets section, choose the the available subnet. 
2. On the Details tab, under Route table, click the provided ID. - The VPC dashboard opens in a new browser tab (or window). 
3. Go to the next step.
<img src="https://imgur.com/3G9dCdg.jpg" height="80%" width="80%" alt="Add route from Marketing VPC to Finance VPC - part 2"/>

---
#### Concept: After a VPC peering connection has been created, traffic cannot flow between the VPCs until routes are established in all associated route tables.
Step 21
1. In the Route tables section, choose the Marketing route table. 
2. On the Routes tab, click Edit routes. 
3. Go to the next step.
<img src="https://imgur.com/vfkbP6d.jpg" height="80%" width="80%" alt="Add route from Marketing VPC to Finance VPC - part 3"/>

---
Step 22
1. Click Add route. 
2. To configure the route, for Destination, in the new search box, type: 172.31.0.0/16 - This CIDR block belongs to the Finance VPC. 
3. For Target, choose Peering Connection. 
4. Below that, choose the peering connection target that contains Marketing <> Finance. 
5. Click Save changes. 
6. Go to the next step.
<img src="https://imgur.com/rLr0yBi.jpg" height="80%" width="80%" alt="Add route from Marketing VPC to Finance VPC - part 4"/>

---
Step 23
1. In the success alert, review the message. 
2. On the Routes tab, review the newly created route. - The new route directs traffic that originates from the Marketing VPC (10.10.0.0/16) intended for the Finance VPC (172.31.0.0/16) through the peering connection. 
3. Go to the next step.
<img src="https://imgur.com/pO2RiId.jpg" height="80%" width="80%" alt="Add route from Marketing VPC to Finance VPC - part 5"/>

---
Step 24
1. Return to the Instances section on the Amazon EC2 console in the other browser tab. 
2. Choose the FinanceServer instance. - You might need to clear the checkbox for the MarketingServer instance. 
3. On the Details tab, scroll down to Subnet ID. 
4. Click the provided ID. 
5. Go to the next step.
<img src="https://imgur.com/sA57A7n.jpg" height="80%" width="80%" alt="Add route from Marketing VPC to Finance VPC - part 6"/>

---
Step 25
1. In the Subnets section, choose the available subnet. 
2. On the Details tab, under Route table, click the provided route ID. - The VPC dashboard opens in a new browser tab (or window). 
3. Go to the next step.
<img src="https://imgur.com/7fFz8EC.jpg" height="80%" width="80%" alt="Add route from Marketing VPC to Finance VPC - part 7"/>

---
#### Concept: Traffic is not able to flow properly between the VPCs if only one VPC's route table is configured. For VPC peering to work effectively, routes must be established in all associated route tables of both VPCs.
Step 26
1. In the Route tables section, choose the Finance route table. 
2. On the Routes tab, click Edit routes. 
3. Go to the next step.
<img src="https://imgur.com/U5GabGx.jpg" height="80%" width="80%" alt="Add route from Marketing VPC to Finance VPC - part 8"/>

---
Step 27
1. Click Add route. 
2. To configure the route, for Destination, in the new search box, type: 10.10.0.0/16 - This CIDR block belongs to the Marketing VPC. 
3. For Target, choose Peering Connection. 
4. Below that, choose the peering connection target that contains Marketing <> Finance. 
5. Click Save changes. 
6. Go to the next step.
<img src="https://imgur.com/SeJJzL7.jpg" height="80%" width="80%" alt="Add route from Marketing VPC to Finance VPC - part 9"/>

---
Step 28
1. In the success alert, review the message. 
2. Review the newly created route. - The new route directs traffic that originates from the Finance VPC (172.31.0.0/16) intended for the Marketing VPC (10.10.0.0/16) through the peering connection. 
3. Go to the next step.
<img src="https://imgur.com/TLJda1q.jpg" height="80%" width="80%" alt="Add route from Marketing VPC to Finance VPC - part 10"/>

---
Step 29
1. Return to the Instances section on the Amazon EC2 console. 
2. Choose the MarketingServer instance. - You might need to clear the check box for the FinanceServer instance. - You might also want to close all other open browser tabs. 
3. Click Connect. 
4. Go to the next step.
<img src="https://imgur.com/FRAKknC.jpg" height="80%" width="80%" alt="Add route from Marketing VPC to Finance VPC - part 11"/>

---
Step 30
1. Click the Session Manager tab. 
2. Click Connect. - The Session Manager terminal for the MarketingServer instance opens in a new browser tab (or window). 
3. Go to the next step.
<img src="https://imgur.com/NndzM52.jpg" height="80%" width="80%" alt="Add route from Marketing VPC to Finance VPC - part 12"/>

---
#### Concept: Peered VPCs do not necessarily accept all data between them. Security features, such as network access control lists and security groups, still apply.
Step 31
1. In the Session Manager terminal window, replacing the current IP address with the IP address that you copied in an earlier step, run: ping <172.31.x.xx> - This is the private IPv4 address is for the FinanceServer instance. 
2. Review to see that the ping command is still not working, 
3. To exit the running process, on your keyboard, press Ctrl+C. 
4. Go to the next step.
<img src="https://imgur.com/Sq6L2PS.jpg" height="80%" width="80%" alt="Ping Finance VPC - 2nd attempt"/>

---
#### Concept: Security groups do not automatically accept data from peered VPCs. You must update security groups to allow a peered VPC as an incoming source.
Step 32
1. In the previous browser, in the Amazon EC2 Instances section, choose the FinanceServer instance. 
2. Click the Security tab. 
3. Under Security groups, click the provided security group name that contains FinanceServerSecurityGroup. 
4. Go to the next step.
<img src="https://imgur.com/eatDMmU.jpg" height="80%" width="80%" alt="Instance security"/>

---
#### Concept: Security groups are stateful. If you send a request from your instance, the response traffic for that request is allowed to flow in regardless of the inbound rules. This also means that responses to allowed inbound traffic are allowed to flow out, regardless of the outbound rules.
Step 33
1. On the Inbound rules tab, click Edit inbound rules. 
2. Go to the next step.
<img src="https://imgur.com/gBC44mb.jpg" height="80%" width="80%" alt="Inbound security - Finance VPC"/>

---
#### Concept: Security group rules are always permissive. You can't create rules that deny access. Using security group rules, you can filter traffic based on protocols and port numbers.
Step 34
1. Click Add rule. 
2. For Type, choose Custom ICMP - IPv4. 
3. For Source, in the new search box, type and choose: 10.10.0.0/16 
4. Click Save rules. 
5. Go to the next step.
<img src="https://imgur.com/CDZRqrh.jpg" height="80%" width="80%" alt="Inbound security rule - Finance VPC"/>
<img src="https://imgur.com/srDTSBJ.jpg" height="80%" width="80%" alt="Inbound rule success alert"/>

---
#### Concept: Your services can communicate after your VPCs are peered and the security groups allow the correct traffic.
Step 35
1. Return to the Systems Manager browser tab. - If the Session Manager command line has timed out, you need to connect to the MarketingServer instance from the Session Manager tab again. 
2. In the terminal window, replacing the current IP address with the IP address that you copied in an earlier step, run: ping <172.31.x.xx> - This private IPv4 address is for the FinanceServer instance. 
3. Review the response data. - The MarketingServer instance should now be able to communicate with the FinanceServer instance. 
4. To exit the running process, on your keyboard, press Ctrl+C. 
5. Go to the next step.
<img src="https://imgur.com/XXjXBvb.jpg" height="80%" width="80%" alt="Successful ping from Marketing VPC to Finance VPC"/>

---
Step 36
Finished!

