# AWS Web Application Firewalls

<img src="/WAF/WAF_structure.PNG">

## Results:
### Only my laptop IP address can access the EC2 Instance
<img src="/WAF/s28.jpg" width="306" height="118">
<img src="/WAF/s18.PNG" width="789" height="372">

### Using a different IP address will result in 403 Forbidden
<img src="/WAF/s27.PNG" width="323" height="109">
<img src="/WAF/s26.PNG" width="756" height="183">

### If attacker try to access ``/admin`` or ``/login`` directory, it will run a CAPTCHA challenge
- This blocked bad bot intrusion attacks

<img src="/WAF/s34.PNG" width="505" height="291"> <img src="/WAF/s35.PNG" width="502" height="287">


# Step-By-Step Procedures

<details>
  <summary>Click to expand</summary>

## Step 1: Create VPC
<img src="/WAF/s1.PNG">

## Step 2: Create Internet Gateway
<img src="/WAF/s2.PNG">

## Step 3: Create Subnet
<img src="/WAF/s3.PNG">

### Subsnet Setting part 1
<img src="/WAF/s4.PNG">

### Subsnet Setting part 2
<img src="/WAF/s5.PNG">

## Step 4: Create a Route table
<img src="/WAF/s6.PNG">

### Set Route table association 
<img src="/WAF/s7.PNG">

### Allow everyone on internet access the resource in the subnet
<img src="/WAF/s8.PNG">

## Step 5: Create a EC2 instance
<img src="/WAF/s9.PNG" width="796" height="832">

### Generate a new key pair if there is not existing one
<img src="/WAF/s10.PNG" width="797" height="499">

### Connnet to existing VPC and subnet, enable public IP
<img src="/WAF/s11.PNG">

### Add a new security rule for port 80 and allow it from anywhere 
<img src="/WAF/s12.PNG">

### Scorll down to advanced details and located to the userdata section, intall the Apache server and create simple web page
- I paste the completed server setup code in "apache_server.txt"

<img src="/WAF/s13.PNG" width="652" height="471">

## Step 5: Create a Target groups
- Connect it to existing VPC
- In next step, select the avaiable instaces and click "include as pending below" button to finished the setup.

<img src="/WAF/s14.PNG">

## Step 6: Create a Application Load Balancer
<img src="/WAF/s15.PNG">

### Configure VPC and subnet setting, and select the security group created in EC2 instance
- In listen and routing section, select the target group we create in step 5

<img src="/WAF/s16.PNG">

### Here is what the Load Balancer looks like after it is successfully created
 - Copy the DNS name show in the Load Balancer page

<img src="/WAF/s17.PNG">

### Paste the DNS address in your browser
- Here is what the webpage looklike if you configure all the setting correctly

<img src="/WAF/s18.PNG" width="789" height="372">

## Step 7: In WAF & Shield, create a Web ACL
<img src="/WAF/s19.PNG">

### In Associated AWS resources section, add the exsiting Load balancer
<img src="/WAF/s20.PNG">

### In this page we can add our own rule and choose default ACL action
- Remain open this page and we going to create a IP set first

<img src="/WAF/s21.PNG">

## Step 8: Open a new tab and go to IP sets under AWS WAF
<img src="/WAF/s22.PNG">

### We going fill to whatever the real IP address of your computeris 
- Here I write down a example IP address, make sure you add /32 at the end of your IP.

<img src="/WAF/s23.PNG">

## Step 9: Go back to the Web ACL page
- In my rules, click add my own rules
- Select IP set as rule type and choose the rule we just created, and select "allow" action

<img src="/WAF/s24.PNG">

### Makre sure you select "block" for Default Web ACL action
- The rest just leave as default and complete the Web ACL configuration

<img src="/WAF/s25.PNG">

## Step 10: Go load balancer and copy the DNS address
- If you try to access the web page in a different IP address, it will show as below

<img src="/WAF/s26.PNG">

### Here I using a different IP address to access the server
<img src="/WAF/s27.PNG" width="323" height="109">

#### Compare to my laptop IP address

<img src="/WAF/s28.jpg" width="306" height="118">

### Go to Web ACLs "Sampled requests"
- You can see a list of allowed and blocked request

<img src="/WAF/s29.jpg" width="306" height="118">

## Step 9: Go back to the Web ACL rules page
- Add a new "my own rule"
- Choose Rule builder as rule type

<img src="/WAF/s30.PNG">

### Here we going to choose OR statment
- Configure all the setting show in the picture below
<img src="/WAF/s31.PNG">

<img src="/WAF/s32.PNG">

### Select CAPTCHA as action
- This rule will protect the server against bad bot intrusion attacks for ``/admin`` or ``/login`` directory

<img src="/WAF/s33.PNG">
</details>
