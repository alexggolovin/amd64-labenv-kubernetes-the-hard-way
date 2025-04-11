# Kubernetes The Hard Way 

# AMD64-Based Infrastructure Lab Provisioning environment configuration example


Verified with all successfully completed steps, mentioned in the latest agnostic ["Kubernetes The Hard Way"](https://github.com/kelseyhightower/kubernetes-the-hard-way) 
official release https://github.com/kelseyhightower/kubernetes-the-hard-way 

[Announced by Kelsey Hightower, in April 2025.
](https://www.linkedin.com/posts/kelsey-hightower-849b342b1_kubernetes-the-hard-way-has-been-updated-activity-7315197014126804992-Qp9R?utm_source=share&utm_medium=member_desktop&rcm=ACoAAAQgt6IBcwC4E7wHLIleC--ia5VFLXJc4mo
)

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








