# Using Longleaf Cluster

## Accessing Longleaf Cluster

First, if you're not on campus network, you need to be connected to the VPN. For setting up the VPN, refer to [this](setting_up_supercomputer_account.md) document. Once Cisco AnyConnect is installed and you have set up 2-step Verification for Duo, you can access the campus network through VPN by connecting to UNCCampus group with your UNC Onyen. Second password should be a Duo command that is either `push`, `sms`, `phone`, or the OTP from the Duo app.

Once you are connected to the campus network, you can ssh to Longleaf on a terminal using the following command

```bash
ssh your_onyen@longleaf.unc.edu
```

You will be prompted to enter your Onyen password, and once you type it in, you should see `[your_onyen@longleaf-login# ~]$` and you are logged onto a log-in node.

## Understanding Longleaf Cluster

There are different nodes on the Longleaf Cluster. First and foremost, there is the log-in node where you will be able to browse through your directories, do scratch work, and submit jobs to compute nodes. 

`Original work by Jinhyun Lee`

`Updated by Jinhyun Lee on 2023-12-01`
