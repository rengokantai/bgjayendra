## EC2 and VPC
- All the EC2 instance types can be launched in a VPC
- Instance types C4, M4 & T2 are available in VPC only and cannot be launched in EC2-Classic
- Launching an EC2 instance within an VPC provides the following benefits
  - Assign static private IP addresses to your instances that persist across starts and stops
  - Assign multiple IP addresses to your instances
  - Define network interfaces, and attach one or more network interfaces to your instances
  - Change security group membership for your instances while they’re running
  - Control the outbound traffic from your instances (egress filtering) in addition to controlling the inbound traffic to them (ingress filtering)
  - Add an additional layer of access control to your instances in the form of network access control lists (ACL)
  - Run your instances on single-tenant hardware

## EC2 Instance IP Addressing
- Private IP address & __Internal DNS Hostnames (Note, not privat HostName)__
  - Private IP address is the IP address that’s not reachable over the internet and can be resolved only within the network
  - When an instance is launched, the default network interface __eth0__ is assigned a private IP address and an internal DNS hostname which resolves to the private
IP address and can be used for communication between the instances in the same network only
  - __Private IP address and DNS hostname cannot be resolved outside the network that the instance is in__
  - Private IP address behaviour
    - remains associated with the Instance when it is stopped or rebooted
    - is disassociated only when the instance is terminated
  - An instance when launched can be assigned a private IP address or 
  EC2 will automatically assign an IP address to the instance within the address range of the subnet
  - __ An additional private IP addresses, known as secondary private IP addresses can also be assigned. Unlike primary private IP addresses, 
  secondary private IP addresses can be reassigned from one instance to another.__
- Public IP address and __External DNS hostnames__
  - A public IP address is reachable from the Internet
  - Each instance assigned a public IP address is also given an External DNS hostname. External DNS hostname resolves to the public IP address outside the network and to the private IP address within the network.
  - __Public IP address is associated with the primary Private IP address through NAT__
  - Within a VPC, an instance may or may not be assigned a public IP address depending upon the subnet Assign Public IP attribute
  - Public IP address assigned to the pool is from the public ip address pool and is assigned to the instance, and not to the AWS account.
  __It cannot be reused once disassociated and is released back to the pool__
  - Public IP address behaviour
    - cannot be manually associated or disassciated with an instance
    - is released when an instance is stopped or terminated. Stopped instance when started receives a new public IP address
    - __is released when an instance is assigned an Elastic IP address__
    - __is not assigned if there are more than one network interface attached to the instance__

- Multipe Private IP addresses
  - In EC2-VPC, multiple private IP addresses can be specified to the instances.
  - This can be useful in the following cases
    - Host multiple websites on a single server by using multiple SSL certificates on a single server and associating each certificate with a specific IP address.
    - Operate network appliances, such as firewalls or load balancers, that have multiple private IP addresses for each network interface.
    - Redirect internal traffic to a standby instance in case your instance fails, by reassigning the secondary private IP address to the standby instance.
  - Multiple IP addresses work with Network Interfaces
    - Secondary IP address can be assigned to any network interface, which can be attached or detached from an instance
    - Secondary IP address must be assigned from the CIDR block range of the subnet for the network interface
    - Security groups apply to network interfaces and not to IP addresses
    - Secondary private IP addresses can be assigned and unassigned to elastic network interfaces attached to running or stopped instances.
    - Secondary private IP addresses that are assigned to a network interface can be reassigned to another one if you explicitly allow it.
    - Primary private IP addresses, secondary private IP addresses, and any associated Elastic IP addresses remain with the network interface when it is detached from an instance or attached to another instance.
    - __Although primary network interface cannot be moved from an instance, the secondary private IP address of the primary network interface can be reassigned to another network interface.__



##Elastic IP Addresses
- An Elastic IP address is a static IP address designed for dynamic cloud computing.
- Elastic IP address can help mask the failure of an instance or software by rapidly remapping the address to another instance 
in your account.
- Elastic IP address is associated with the AWS account, not a particular instance, 
and it remains associated with the account until released explicitly
- When an instance is launched in the default vpc, it is assigned 2 IP address, 
a private and a public IP address which is mapped to the private IP address through NAT
- __An instance launched in a non default vpc, it is assigned only a private IP address__ unless a public address is specifically requested or the subnet public ip attribute is enabled
- For an instance, without a public IP address, to communicate to internet it must be assigned an Elastic IP address
- When an Elastic IP address is assigned to an instance, the public IP address is disassociated with the instance and when the Elastic IP address is dissociated the public IP address is assigned back to the instance. However, if the instance is attaced a secondary network interface, the public IP address is not automatically assigned
- Elastic IP addresses are not charged when associated with a running instance
- However, Amazon imposes a small hourly fee for an unused Elastic IP address to ensure efficient use of Elastic IP addresses. So you will be charged for an Elastic IP address if it is not associated or associated to an instance in stopped state or associated to an unattached network interface.
- __All AWS accounts are limited to 5 EIPs (soft limit), because public (IPv4) Internet addresses are a scarce public resource__



### Elastic Network Interfaces (ENI)
- Elastic Network Interfaces (ENIs) are virtual network interfaces that can be attached to the instances running in an VPC only
- ENI consists of the following
  - A primary private IP address.
  - __One or more secondary private IP addresses__
  - One Elastic IP address per private IP address.
  - One public IP address, which can be auto-assigned to the elastic network interface for eth0 when an instance is launched, 
  but only when elastic network interface for eth0 is created instead of using an existing network interface.
  - One or more security groups.
  - A MAC address.
  - A source/destination check flag.
  - A description.
- __ENI can be attached to an instance, detached from that instance and attached to an other instance.__ 
__Attributes of an ENI like elastic IP address, private IP address follow the ENI and when moved from one instance to an other
instance all traffic to the ENI will be routed to the new instance.__
- An instance in a VPC always has a default primary ENI attached (eth0) 
which has a private ip address assigned from vpc range and cannot be detached.
- Additional ENI can be attached to the instance and the number varies depending upon the instance type
- Multiple elastic network interfaces to an instance are useful when you want to:
  - Create a management network.
  - Use network and security appliances in your VPC.
  - Create dual-homed instances with workloads/roles on distinct subnets.
  - Create a low-budget, high-availability solution.
- ENI Best Practices
- You can attach an elastic network interface to an instance when it’s running (hot attach), when it’s stopped (warm attach), or when the instance is being launched (cold attach).

- You can attach an elastic network interface to an instance when it’s running (hot attach), when it’s stopped (warm attach), or when the instance is being launched (cold attach).
- You can detach secondary (ethN) elastic network interfaces when the instance is running or stopped. However, __you can’t detach the primary (eth0) interface.__
- You can attach an elastic network interface in one subnet to an instance in another subnet in the same VPC; however, both the elastic network interface and the instance must reside in the same Availability Zone.
- When launching an instance from the CLI or API, you can specify the elastic network interfaces to attach to the instance for both the primary (eth0) and additional elastic network interfaces.
- Launching an Amazon Linux or Microsoft Windows Server instance with multiple network interfaces automatically configures interfaces, private IP addresses, and route tables on the operating system of the instance.
- A warm or hot attach of an additional elastic network interface may require you to manually bring up the second interface, configure the private IP address, and modify the route table accordingly. Instances running Amazon Linux or Microsoft Windows Server automatically recognize the warm or hot attach and configure themselves.
- Attaching another elastic network interface to an instance is not a method to increase or double the network bandwidth to or from the dual-homed instance.
