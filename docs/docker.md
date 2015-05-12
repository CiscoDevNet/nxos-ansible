## Get Your Docker On

**Step 1 - Download Docker Image**

Ansible does not require a dedicated server to be used.  In fact, many machines could have Ansible installed and they can be used to simultaneuously automate any given environment (not recommending that here, but it's definitely a nice option to have). 

> Note: This option assumes you already have Docker installed.  If you don't, follow the instructions that can be found [here](https://docs.docker.com/installation/).

The Docker image will come ready to go with Ansible along with the Cisco dependencies required to start automating Nexus environments.

This [YouTube video](https://www.youtube.com/watch?v=ftxBCCKbVn4) goes through this process.

```
sudo docker pull jedelman8/nxos-ansible
```

Make sure you see the new image
```
sudo docker images
```

Start your first container for automating networks!
```
sudo docker run -it jedelman8/nxos-ansible
```

**Step 2 - Update hosts File**

Ensure you can ping your Nexus NX-API enabled switches by name (not required, but helpful) from the new container.  Here is a sample from a test container.  This shows the file after adding two new entries for `n9k1` and `n9k2`.

```
root@3b5d22fa1231b:~$ cat /etc/hosts
172.17.0.23      3b5d22fa1231b
127.0.0.1        localhost

#### Add your Nexus Switches here
#### MUST Use the MGMT0 IP Address

10.1.10.100      n9k1
10.1.10.101      n9k2

```

`vim` is already installed in the container, so feel free to use it to modify the `hosts` file.

You should now be able to `ping n9k1` to get a response back from the MANAGEMENT IP address of the Nexus switch.


**Step 3 - Update Authentication File**

**Edit the file**
```
root@3b5d22fa1231b:~$ sudo vim .netauth
```

Make changes using a text editor (example above is using vim) such that it follows the format below.  Your file MUST continue to look the one provided (*just insert the username and password for your switches*).

```
# the .netauth file
# make sure you input the proper creds for your device
---

cisco:
  nexus:
    username: "cisco"
    password: "cisco"

```


If the username and password differs for a switch or group of switches, you must then use the username and password parameters in the Ansible playbook for each task.  This can be seen in the **Automated Data Collection** playbook examples that be found below.

> Note: if you are using Ansible Tower, you'll need to disable a security setting to allow the use of .netauth to work, i.e. the reading in of values of a file that exists outside of your working directory.
> 
> Note: this section will be updated over time to include instructions for using `ansible-vault`.
