# Differece between the windows and Linux

Windows
-------------------
1) Heavy graphics
2) More cost
3) slow
4) heavy resource consumption
5) performance -> frequest restarts ( other wise laptop will become stuck)
6) security -> Anti virus installed is must
7) upgrate to other versions is little tough


Linux
--------------------------
1) open source
2) freee
3) no graphics, only text
4) supper fast
5) less resources -> 9mb of the OS can be installed
6) run for years with out restarts
7) By default security provided in OS
8) upgrate is easy



AWS Regions and Avaliability Zones
----------------------------------------------
1) North virginia is the first AWS region . so cost is very less compared to any other region. 
     us-east-1

2) Avaliability zones (Data centers) => meaning our servers are present over there.
    -> Every regain have atleat 2 avliabity zones for backeup purpose.
    -> In HYD 3 data cenetrs are there.



Launch instance
------------------------
 => AMI (amazon machine image) we need to select i.e OS.

 1) we need to create firewall i.e security groups
 2) we need to generate the SSH keys  by using this command (ssh-keygen -f <file-name>)
 3) then launch the instance


Firewall and authentication
----------------------------

## Authentication

1) What you know?   -> User and password
2) What you have?   -> username  , RSA/OTP/ Goggle auth/ microsoft auth
3) What you are?    -> fingure prints, retina, palm, etc.


# Linix Server , How to connect?
  -> we need clint software -> putty, mobaxterm, gitbash, windows CMD, Mac terminal.

  -> Git bash is one of the SSH client.

  -> Git bash you can run the linux commands in windows.

  windows -> \  (backward)
  linux -> / (forward)


# generating the SSH key

  => ssh-keygen -f <file-name>    (file-name is nothing but your ssh key file).

        -> By this command we will get the 2 files sored in our directory 
            1) private and public key
        -> we need to give public key to aws while creating the instance.

Firewall setup == security groups
----------------------
-> you can think of AWS Security Groups as a kind of virtual firewall,
-> They control inbound and outbound traffic to AWS resources (like EC2, RDS, Lambda with VPC).
✅ Similar to a firewall: Controls who can talk to your server and on what ports.

✅ Easier: You don’t manage complex rules, just define allow rules (no deny rules).
-> Incoming and outgoing  (inbound and outbound trafic)



# How to connect to server using the gitbash
------------------------------------------------------------
->Take the public ip address = 13.221.76.80

-> userName by default => ec2-user

-> Command to connect to server using the git bash => ssh -i <private-key file name> ec2-user@<server ip address>