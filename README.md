Secure VPC Architecture with Public and Private Subnets for Production Environment

Overview :
This project's overview is depicted in the diagram below. The setup revolves around a Virtual Private Cloud (VPC) featuring both public and private subnets, thoughtfully distributed across two Availability Zones to ensure reliability.

Within each public subnet, there's a NAT gateway to facilitate outbound internet connectivity and a load balancer node for effective traffic distribution.

On the other hand, the project's servers reside in the private subnets. Their deployment and termination are automated through an Auto Scaling group, allowing them to dynamically adapt to workload changes. These servers play a pivotal role in receiving traffic from the load balancer and can access the internet through the NAT gateway when necessary

image

Steps :
Step 1 :
Create the VPC :
Open the Amazon VPC console by visiting https://console.aws.amazon.com/vpc/.
On the dashboard, click on "Create VPC."
Under "Resources to create," select "VPC and more."
Configure the VPC:
a. Provide a name for the VPC in the "Name tag auto-generation" field.
b. For the IPv4 CIDR block, leave it as default suggestion.
Configure the subnets:
a. Set the "Number of Availability Zones" to 2 for increased resiliency across multiple Availability Zones.
b. Specify the "Number of public subnets" as 2.
c. Specify the "Number of private subnets" as 2.
d. For NAT gateways, choose "1 per AZ" to enhance resiliency.
g. For VPC endpoints, you can choose "None" .
h. Regarding DNS options, clear the checkbox for "Enable DNS hostnames."
Once you've configured all the settings, click "Create VPC."

p1

p2

p3

p4 6. Now you can see you are successfully Created VPC .

Step 2:
Creating the Auto Scaling Group :
p5

p6

p7

p8

p8

p9

Now you have to choose the Key-pair you created.
p9

p11

p12

p13 2. Scroll Down and then Click "Next".

p14 3. Scroll Down and then Click "Next".

p15 4. Scroll Down and then Click "Next".

p16 5. Scroll Down and then Click "Skip to Review".

p17 6. Now your are Successfully Created Auto Scaling Group.
7. Open the AWS Management Console.
8. Navigate to the EC2 console by clicking on "Services" in the top-left corner, then selecting "EC2" under the "Compute" section.
9. In the EC2 dashboard, you'll find the "Instances" link on the left-hand navigation pane. Click on "Instances."
10. Here, you should see the list of EC2 instances associated with your account. Look for the instances created by your Auto Scaling Group.
Since you mentioned that the Auto Scaling Group launched instances in different AZs, you can check the "Availability Zone" column to verify that these instances are indeed distributed across multiple AZs.

Step 3 :
Creating the Bastion Host :
Launch Instance as Specified below .
p18

p19

p20

p21`

p22

Step 4:
SSH into Private Instance
SSH into the Bastion Host Instance: To SSH into the private instances, we first need to connect to our Bastion host instance. From there, we'll be able to SSH into the private instance.
Ensure the PEM File is Present on the Bastion Host: Additionally, make sure that the PEM file is present on the Bastion host. Without it, you won't be able to SSH into the private instance from the Bastion host.
Open a Terminal: Open a terminal window on your local machine.
Execute the Following Commands: a. If your PEM file is named something like <aws demo.pem>, you must remove spaces in the filename. Please rename the file to something like <aws_demo.pem>. b. Copy the PEM file to the Bastion host using the scp command. Replace <pem file location> with the local and remote file paths, and <bastion host public IP> with the Bastion host's public IP address. Example:
scp -i /Users/mathesh/Downloads/aws_demo.pem /Users/mathesh/Downloads/aws_demo.pem ubuntu@34.229.240.123:/home/ubuntu
c. The above command will copy the PEM file from your computer to the Bastion host. Once the file is successfully copied, move on to the next step. d. SSH into the Bastion host using the following command:
ssh -i aws_demo.pem ubuntu@34.229.240.123
e. After SSHing into the Bastion host, use the ls command to check if the aws_demo.pem file is present. If it's not there, double-check your previous commands. f. Now, you can SSH into the private instance using the following command, replacing <private IP> with the private instance's IP address:
ssh -i aws_demo.pem ubuntu@<private IP>
g. We will deploy our application on one of the private instances to test the load balancer. h. After successfully SSHing into the private instance, create an HTML file using the Vim text editor:
vim demo.html
i. This will open the Vim editor. Copy and paste any HTML content you like into the editor. j. For example:
<!DOCTYPE html>
<html>
<head>
<title>Page Title</title>
</head>
<body>

<h1>This is an AWS Demo Production</h1>
</body>
</html>
k. After pasting the content, save the file by pressing 'Esc' to exit insert mode and then entering :w to save. l. Finally, start a Python HTTP server on port 8000 to deploy your application on the private instance:
python3 -m http.server 8000
Now, your application is deployed on the private instance on port 8000.

Note :
We intentionally deployed the application on only one instance to check if the Load Balancer will distribute 50% of the traffic to one instance (which will receive a response) and 50% to another instance (which will not receive a response).

Step 4 :
Creating the Load Balancer :
Access the EC2 Terminal.
Follow the steps outlined below.
lb1

lb2

lb3

lb4

lb5

lb6

lb7

lb8

lb9

lb10

lb11

lb12

lb13

lb14

lb15

lb16

Now We Successfully deployed Application securely in Private instance , We can access it through Internet using Load Balancer Securely .
