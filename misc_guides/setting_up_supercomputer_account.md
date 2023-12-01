From [https://help.rc.unc.edu/longleaf-cluster/](https://help.rc.unc.edu/longleaf-cluster/)
>The Longleaf cluster is a Linux-based computing system available to researchers across the campus free of charge. With nearly 6500 conventional compute cores delivering 13,000 threads (hyperthreading is enabled) and a large, fast scratch disk space, it provides an environment that is optimized for memory and I/O intensive, loosely coupled workloads with an emphasis on aggregate job throughput over individual job performance. In particular, workloads consisting of a large number of jobs each requiring a single compute host are best suited to Longleaf. The Longleaf cluster is targeted for data science and statistical computing workloads, very large memory jobs, and GPU computing. Resources are managed through a fair-share algorithm using SLURM as the resource manager/job scheduler.

To get access to the Longleaf Cluster, refer to this ([https://help.rc.unc.edu/request-a-cluster-account](https://help.rc.unc.edu/request-a-cluster-account)) page.

On the request form, your departmental affiliation would be **Applied Physical Sciences** and the rest should be straightforward.

Once your longleaf account is set up, you can ssh into the cluster on a terminal
```bash
ssh your_onyen@longleaf.unc.edu
```

If you are on campus network, it should connect right away and prompt you for your onyen password. If you are not, you should connect to the campus VPN first. For VPN setup, refer to [https://portal.ed.unc.edu/resources/connecting-to-vpn-for-off-campus-access-to-network-resources/#4-setting-up-a-vpn-connection](https://portal.ed.unc.edu/resources/connecting-to-vpn-for-off-campus-access-to-network-resources/#4-setting-up-a-vpn-connection). Once you have the VPN connection, ssh should work fine.

In order to gain access to the GPU nodes, you need to send an email to [research@unc.edu](mailto:research@unc.edu?subject=GPU%Access/QoS%on%Longleaf%Cluster) with the subject line "GPU Access/QoS on Longleaf Cluster." Copy Dr. Daphne Klotsa on the email and explain to ITS that you need access to the GPU nodes in order to do your research. Once they respond and confirm that you have access to the GPU node, test it by requesting a node on the Cluster using
```bash
srun --ntasks=1 --cpus-per-task=8 --mem=8G --time=8:00:00 --partition=gpu --gres=gpu:1 --qos=gpu_access --pty /bin/bash
```

If you indeed do have access, you should either be assigned a node immediately or be queued for the use.