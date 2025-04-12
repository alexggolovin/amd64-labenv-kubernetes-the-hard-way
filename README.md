# Kubernetes The Hard Way 

# AMD64-Based Infrastructure Lab Provisioning environment configuration example
Verified with all successfully completed steps, mentioned in the latest agnostic ["Kubernetes The Hard Way"](https://github.com/kelseyhightower/kubernetes-the-hard-way) 
official release https://github.com/kelseyhightower/kubernetes-the-hard-way 

[Announced by Kelsey Hightower, in April 2025.
](https://www.linkedin.com/posts/kelsey-hightower-849b342b1_kubernetes-the-hard-way-has-been-updated-activity-7315197014126804992-Qp9R?utm_source=share&utm_medium=member_desktop&rcm=ACoAAAQgt6IBcwC4E7wHLIleC--ia5VFLXJc4mo
)

### Copyright

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.




### Documentation Release Kubernetes Component versions:

kubernetes v1.32.x
containerd v2.1.x
cni v1.6.x
etcd v3.6.x


### Dependencies for infrastructure provisioning:

1. Debian based Linux distribution (tested on Kali Linux):
PRETTY_NAME="Kali GNU/Linux Rolling"
NAME="Kali GNU/Linux"
VERSION_ID="2025.1"
VERSION="2025.1"
VERSION_CODENAME=kali-rolling
ID=kali
ID_LIKE=debian
HOME_URL="https://www.kali.org/"
SUPPORT_URL="https://forums.kali.org/"
BUG_REPORT_URL="https://bugs.kali.org/"
ANSI_COLOR="1;31"


2. Vagrant Tool for building and distributing virtualized development environments
   ( tested on 2.3.7+git20230731.5fc64cde+dfsg-3+b1 installed from official distribution repository)


3. Virtualbox virtualization solution, tested on version 7.0.20-dfsg-1.2, installed from official distro repository.


### How To start with Lab Provisioning

1. Install dependencies on Debian-based linux system of your choice (please check package version requirements), using commands:

```bash
apt-get update
```

```bash
apt-get install vagrant virtualbox
```

2. Clone provisioning repository content into your working home directory
```bash
git clone https://github.com/alexggolovin/amd64-labenv-kubernetes-the-hard-way.git
```

3. Run infrastructure provisioning commands from new cloned repository folder:
```bash
vagrant up
```

4. Command running examples:

 4.1 Infrastructure status check:
```bash
─$ vagrant status    
Current machine states:

k8s_server                not created (virtualbox)
k8s_node_0                not created (virtualbox)
k8s_node_1                not created (virtualbox)
k8s_jumpbox               not created (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
```

 4.2 Infrastructure provisioning:
```bash
vagrant up
```

```bash
─$ vagrant status
Current machine states:

k8s_server                running (virtualbox)
k8s_node_0                running (virtualbox)
k8s_node_1                running (virtualbox)
k8s_jumpbox               running (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
```

4.3 Infrastructure destroy
```bash
vagrant destroy -f
```

5. Login to the new provisioned jumpbox vm to begin with the "Kubernetes Hard Way" journey:
```bash
ssh -i id_rsa root@jumpbox.kubernetes.local
```

 More detailed real life command running output examples can be found in "[My_Hard_Way_Journey](My_Hard_Way_Journey)", 
work log tracking part:



Next: [My_Hard_Way_Journey](My_Hard_Way_Journey)



 






