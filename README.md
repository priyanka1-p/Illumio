# Illumio

# Illumio

# AWS Flow Log Parser

This program parses AWS VPC flow log data and maps each log entry to a tag based on a provided lookup table. The lookup table is a CSV file that defines the combination of destination port (`dstport`) and protocol that corresponds to specific tags.

### Features:
- **Supported Flow Log Version**: Version 2 (Default format with 14 fields).
- **Protocol Handling**: Supports numeric protocol numbers mapped to common protocol names (TCP, UDP, etc.).
- **Tag Mapping**: Matches log entries to tags based on the `dstport` and protocol.
- **Untagged Handling**: Logs without a corresponding tag in the lookup table are marked as "Untagged."
- **Detailed Output**: Generates two output CSV files:
  - `tag_counts.csv`: Counts of how many times each tag was applied.
  - `port_protocol_counts.csv`: Counts of how many times each port/protocol combination was found.

---

## Requirements

- **Python**: Version 3.8 or higher
- **Required Libraries**: Install the required libraries using `pip`


```bash
pip install pandas

## Command to Run
python flow_log_parser.ipynb --log_file <path_to_flow_log_file> --lookup_table <path_to_lookup_table>

Steps to Create a VPC Setup
Create a VPC:

Go to the VPC dashboard in the AWS Management Console.
Click on Your VPCs in the left navigation pane.
Click on Create VPC.
Enter a name for your VPC and specify the IPv4 CIDR block (e.g., 10.0.0.0/16).
Click on Create VPC.
Create Subnets:

In the VPC dashboard, click on Subnets in the left navigation pane.
Click on Create subnet.
Select the VPC you just created.
Enter a name and specify the IPv4 CIDR block for the subnet (e.g., 10.0.1.0/24).
Repeat this step to create additional subnets as needed.
Create an Internet Gateway:

Click on Internet Gateways in the left navigation pane.
Click on Create internet gateway.
Name your internet gateway and click Create.
After creating the internet gateway, select it and click on Actions > Attach to VPC.
Choose your VPC and click Attach.
Create a Route Table:

Click on Route Tables in the left navigation pane.
Click on Create route table.
Name your route table and select the VPC you created.
Click Create.
Select the route table, click on the Routes tab, and then click on Edit routes.
Click on Add route, enter 0.0.0.0/0 for the destination, and select the internet gateway you created as the target.
Click Save routes.
Associate Subnets with the Route Table:

Select your route table and click on the Subnet Associations tab.
Click on Edit subnet associations and select the subnets you want to associate with this route table.
Click Save associations.
Launch an EC2 Instance:

Go to the EC2 dashboard in the AWS Management Console.
Click on Launch Instance.
Choose an Amazon Machine Image (AMI) and select an instance type.
In the Configure Instance step, ensure you select the VPC and the subnet you created.
Click through the remaining steps to configure security groups, key pairs, and launch your instance.
Enable VPC Flow Logs:

After creating your VPC and EC2 instance, follow the instructions in the previous section to enable flow logs and send them to CloudWatch.

Assumptions
Supports Version 2 Only: The program only accepts AWS VPC Flow Logs that are in the default Version 2 format with exactly 14 fields.
Default Log Format: The program is built for the standard flow log format and does not support custom field configurations.
Untagged Handling: If a dstport and protocol combination does not exist in the lookup table, the log is counted as "Untagged."
Protocol Mapping: The program automatically maps common protocol numbers (e.g., 6 for TCP, 17 for UDP) to their respective names.


Tests Performed
1. Valid Logs (Version 2):
Tested the parser with valid AWS VPC Flow Logs in Version 2 format to ensure correct parsing of fields.
Verified the correct matching of logs to tags using different destination ports and protocols.
2. Invalid Logs:
Rejected flow logs that had more than 14 fields (used for Version 3 logs or custom formats).
Rejected flow logs with missing fields or incorrectly formatted entries.
3. Lookup Table Matching:
Verified that the program correctly applies tags from the lookup table and counts untagged entries.
Tested different lookup table entries to ensure accuracy.
