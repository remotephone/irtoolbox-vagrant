This is a simple toolbox for incident response. It includes a local installation of splunk, cyberchef, and some other useful tools to get started. Default username/password is ubuntu/ubuntu so change that. If you see some parts unused in the config, it's because this is a work in progress.

This Vagrantfile depends on a slightly modified version of the [Splunk Enterprise Docker image](https://github.com/splunk/docker-splunk/blob/master/enterprise/README.md) they were so generous to write for us all. I modified free space requirements and enabled HTTPS by default, but I don't think you'd have any problems swapping mine out for theirs. 

Tested on Windows 10 v17074 with Virtualbox 5.2.6 and Vagrant 2.0.1. Other stuff might work, I dunno.

### Requirements

Install Virtaulbox, Vagrant, and these plugins:

```
vagrant plugin install vagrant-disksize
vagrant plugin install vagrant-reload
```
