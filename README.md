# Automating Incident Response with Splunk SOAR

## Introduction

SOAR SOAR SOAR. You keep hearing it but what does it mean??

When I first heard this term I pondered as to how it fit into the world of Incident Response and the SOC. In order to learn about it I decided to implement it at our SOC.

## [Splunk SOAR](https://www.splunk.com/en_us/products/splunk-security-orchestration-and-automation.html) (Formerly Splunk Phantom)

### Requirements

Download and Install [Splunk SOAR](https://www.splunk.com/en_us/download/soar-free-trial.html?locale=en_us). For our install we installed it onto a virtual machine running Centos.

<aside>
💡

> You cannot install Splunk SOAR and Splunk Enterprise Server on the same device.
> 
</aside>

You will also need to install the following apps on your Splunk Server:

- [Splunk App for SOAR](https://splunkbase.splunk.com/app/6361) for receiving events from Splunk SOAR
- [Splunk App for SOAR Export](https://splunkbase.splunk.com/app/3411) to forward events to Splunk SOAR

## Playbooks

Once you have everything working and are able to forward your events to Splunk SOAR we can start the fun part of automating tasks. 

Playbooks are a set of instructions and actions that Splunk SOAR can do on a given input. For our use case we forwarded unidentified IP addresses that were flagged by our detection and sent it to a custom playbook called UID Checker.

![Screenshot of SOAR playbooks](https://prod-files-secure.s3.us-west-2.amazonaws.com/54d771b3-055a-4b5d-953e-db2a04b36e20/6551a4e2-482e-45b3-a130-99c3e002eced/image.png)

Screenshot of SOAR playbooks

As a Security Analyst one of the things I would do when given an unidentified IP is run it through VirusTotal(VT). Every time a new IP is flagged I would browse to VT, copy the IP and paste it into the search and run it to see the results. 

![Screenshot of VirusTotal scan results](https://prod-files-secure.s3.us-west-2.amazonaws.com/54d771b3-055a-4b5d-953e-db2a04b36e20/10f5b22f-874d-4253-a46d-d9d8e8424263/malicious_IP.png)

Screenshot of VirusTotal scan results

This gets pretty repetitive. Imagine doing this for 100s of IPs addresses throughout the month. This is where Splunk SOAR helps us to automate this process and returning the results via an event in Splunk. Splunk playbooks are created via a GUI interface and we can filter incoming data based on certain fields and run actions on them. Splunk SOAR has a built in VirusTotal action that scans ip addresses. Below is an example of the workflow of the playbook we created. 

![Screenshot of Splunk SOAR playbook](https://prod-files-secure.s3.us-west-2.amazonaws.com/54d771b3-055a-4b5d-953e-db2a04b36e20/c2005c7b-05bc-41fc-8707-14f6ddf650c7/image.png)

Screenshot of Splunk SOAR playbook

We recently also integrated a WHOIS command to add more data on incoming IP addresses. You can see the corresponding output in the screenshot below.

![Screenshot of playbook automation ](https://prod-files-secure.s3.us-west-2.amazonaws.com/54d771b3-055a-4b5d-953e-db2a04b36e20/37016eae-0f5f-4e50-b776-7928c920158d/image.png)

Screenshot of playbook automation 

This information is further exported to Splunk via the Splunk app for SOAR and you can see the data below.

![Splunk SOAR data seen from Splunk](https://prod-files-secure.s3.us-west-2.amazonaws.com/54d771b3-055a-4b5d-953e-db2a04b36e20/4833b844-6c41-43ff-8933-2ce33b6ee919/image.png)

Splunk SOAR data seen from Splunk

## Conclusion

Splunk SOAR helps us save time by automating the repetitive tasks that the Security Analysts have to do. This saves time for the Security Analysts so that they can spend time elsewhere. There is a lot more to explore with Splunk SOAR and I am currently looking into Splunk SOAR automation using SSH to potentially add firewall block rules for IPs that are malicious.
