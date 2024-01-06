# Using Longleaf Cluster

## Accessing Longleaf Cluster

First, if you're not on campus network, you need to be connected to the VPN. For setting up the VPN, refer to [this](setting_up_supercomputer_account.md) document. Once Cisco AnyConnect is installed and you have set up 2-step Verification for Duo, you can access the campus network through VPN by connecting to UNCCampus group with your UNC Onyen. Second password should be a Duo command that is either `push`, `sms`, `phone`, or the OTP from the Duo app.

Once you are connected to the campus network, you can ssh to Longleaf on a terminal using the following command

```bash
ssh your_onyen@longleaf.unc.edu
```

You will be prompted to enter your Onyen password, and once you type it in, you should see `[your_onyen@longleaf-login# ~]$` and you are logged onto a log-in node.

## Understanding Longleaf Cluster

There are different nodes on the Longleaf Cluster. First and foremost, there is the log-in node where you will be able to browse through your directories, do scratch work, and submit jobs to compute nodes. Note that this node is not the node where a heavy computation should be done. It's only an interface for you to access the cluster and is not designed for computations.

For computations, there are the computation nodes where your simulations should run on. these nodes can have singular or multiple CPU(s) and GPU(s). For HOOMD simulations, you should be allocating 1 GPU per simulation unless you have specifically designed your simulation with MPI domain decomposition (not discussed in this manual) and to be run on multiple GPUs.

For CPU access, you should be using the `general` partition and for GPU, `gpu` will use GTX 1080 while `volta-gpu` will use Volta architecture GPUs. You may be able to get your jobs allocated to the `beta-gpu` where the new Amphere architecture GPUs are. If you have no idea what these architectures mean, just submit everything to the `gpu` partition to be safe. In theory, Volta and Amphere GPUs are supposed to be newer and faster but you always have a chance to get your jobs queued behind other submitted jobs.



`Original work by Jinhyun Lee`

`Updated by Jinhyun Lee on 2024-01-06`
