
# Puppet Master and Puppet Agent Setup on AWS

This guide will help you set up a Puppet Master and Puppet Agent (or Puppet Node) on Amazon Web Services (AWS) using straightforward commands. This setup allows you to manage configurations and automate tasks on your nodes using Puppet.

MY Video link:-
https://www.youtube.com/watch?v=bMBue_LOW7g&t=578s

## Puppet Master Setup

### Modify the Puppet Master's hosts file

```
sudo nano /etc/hosts
```

Add the following line:

```
<Puppet_Master_Private_IP> puppet
```
![p1](https://github.com/vishal815/Puppet_Master_and_Puppet_slave_setup_on_AWS/assets/83393190/0f5f7485-7f75-4c5d-87f3-41ddfb1185b0)


### Download and install the Puppet Master packages

```
curl -O https://apt.puppetlabs.com/puppet6-release-bionic.deb
```
```
sudo dpkg -i puppet6-release-bionic.deb
```
```
sudo apt-get update
```
```
sudo apt-get install puppetserver -y
```
## Add changes
```
sudo nano /etc/default/puppetserver
```
```
JAVA_ARGS = "-Xms512m -Xms512m"
```
### Allow traffic on port 8140

```
sudo ufw allow 8140
```

### Configure and start Puppet Master

```
sudo systemctl enable puppetserver.service
```
```
sudo systemctl start puppetserver.service
```

### Verify the version of Puppet Master

```
sudo systemctl status puppetserver.service
```

## Puppet Agent Setup

### Modify the Puppet Agent's hosts file

```
sudo nano /etc/hosts
```

Add the following line:

```
<Puppet_Master_Private_IP> puppet
```

### Download and install the Puppet Agent packages

```
curl -O https://apt.puppetlabs.com/puppet6-release-bionic.deb
```
```
sudo dpkg -i puppet6-release-bionic.deb
```
```
sudo apt-get update
```
```
sudo apt-get install puppet-agent -y
```

### Enable and restart the Puppet Agent service

```
sudo systemctl enable puppet
```
```
sudo systemctl restart puppet
```
```
sudo systemctl status puppet
```

## Signing Certificates on the Puppet Master

### List certificate requests

```
sudo /opt/puppetlabs/bin/puppetserver ca list
```

### Sign a specific certificate request

```
sudo /opt/puppetlabs/bin/puppetserver ca sign --certname <Agent_CertName>
```
![ss](https://github.com/vishal815/Puppet_Master_and_Puppet_slave_setup_on_AWS/assets/83393190/8d1eaf2d-9ed0-4987-add2-289be93a7ebd)


## Applying Manifests on Puppet master

### Create a Puppet manifest file

```
sudo nano /etc/puppetlabs/code/environments/production/manifests/site.pp
```

Add content like the following example:

```
file { '/tmp/puppet_test.txt':
  ensure => present,
  mode   => '0644',
  content => "Hello from Puppet master (Welcome to puppet testing with vishal.) to agent on IP address ${ipaddress_eth0}\n",
}
```

### Trigger Puppet Agent to apply the changes (run on puppet Agent)

```
sudo /opt/puppetlabs/bin/puppet agent --test
```

### Check the content of the test file (run on puppet Agent)

```
sudo cat /tmp/puppet_test.txt

```
# 👉
[download PPT for All command. ](https://github.com/vishal815/Puppet_Master_and_Puppet_slave_setup_on_AWS/files/12306792/Simplify-your-Puppet-Master-and-Puppet-slave-setup-on-AWS-with-these-straightforward-commands.pptx)


Congratulations! Your Puppet Master and Puppet Agent are now set up and running. You've tested the configuration to ensure everything is working smoothly.

![Puppet-Cheat-Sheet (1)-PhotoRoom](https://github.com/vishal815/Puppet_Master_and_Puppet_slave_setup_on_AWS/assets/83393190/b666ad4d-ac19-401b-ad76-54921826906a)


![chef-vs-puppet-vs-ansible-what-are-the-differences-it-infographic](https://github.com/vishal815/Puppet_Master_and_Puppet_slave_setup_on_AWS/assets/83393190/f3cf14d2-e39e-49ad-b514-634a8b475099)

Happy learning!


### script and instructions for setting up a Puppet manifest on the Master node and applying it on the Slave node. Here's a cleaned-up version of your instructions:

### Master Node:
## 1.Navigate to the Puppet directory:
```
cd /etc/puppetlabs
```
## 2.List the contents:
```
ls
```
## 3.Navigate to the code directory:
```
cd code
```
## 4.List the contents:
```
ls
```
## 5.Create the necessary directories:
```
sudo mkdir -p environments/production/manifests/
```
## 6.Create the necessary directories:
```
cd environments/production/manifests/
```
## 7.Create a new manifest file (`new_site.pp`):
```
sudo nano new_site.pp
```
## 8.Add the following content to the new_site.pp file:
```
node default {
    # 1st resource: Install Nginx
    package {'nginx':
        ensure => installed,
    }

    # 2nd resource: Create a status file
    file{ '/tmp/status.txt':
        content => 'Nginx has been installed successfully',
        mode    => '0644',
    }
}
```
## 9.Add the following content to the new_site.pp file:
# .Press Ctrl + X to exit.
# .Press Y to confirm saving.
# .Press Enter to save the file with the same name.

### Slave Node:
## 1.Apply the Puppet catalog:
```
sudo /opt/puppetlabs/bin/puppet agent --test
```
## 2.Verify Nginx installation:
### .Copy the public IP of the slave node.
### .Open a web browser and paste the IP.
### .Hit `Enter` to check if it shows the Nginx default page.
## 3.Check the content of the status file:
```
cat /tmp/status.txt
```
### .On the slave node, run:
```
cat /tmp/status.txt
```
### .It should display: Nginx has been installed successfully.
### This should guide you through setting up your Puppet manifest and verifying its application on the slave node.



