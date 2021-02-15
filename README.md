# syft-hacks
Security research on OpenMined repositories. Part of the PySyft and PyGrid core security audit project.

[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)

## Table of Contents

- [Background](#background)
- [Install](#install)
- [Usage](#usage)      
- [Contributing](#contributing)
- [License](#license)

## Background
Created to provide a starting point for the core security audit of OpenMined libraries.

## Install

### Kali Linux on VMWare Workstation
Use this setup for automated repository and virtual environment testing (safety, Trivy, Bandit, etc)

1. Download and install [VMWare Workstation](http://www.vmware.com/go/tryworkstation-win)
2. Activate [VMWare Workstation](http://www.vmware.com/go/tryworkstation-win) with an [Open-Source license](https://my.vmware.com/en/web/vmware/downloads/details?downloadGroup=WKST-1610-OSS&productId=1038)
3. Download a compatible [Kali Linux VMWare image](https://www.offensive-security.com/kali-linux-vm-vmware-virtualbox-image-download/) and **load it on VMWare**
4. Inside your Kali Linux, [Make Python3 the default Python](https://thequickblog.com/how-to-change-default-version-of-python-as-python3/)
5. Install [virtualenv](https://pypi.org/project/virtualenv/) to avoid [Anaconda](https://www.anaconda.com/) and messing with Kali's Python settings.
6. Make a virtual environment with [virtualenv](https://pypi.org/project/virtualenv/) and activate it with the `source` command, mine looks like this:

     `$ source /home/kali/syft_venv/sec_audit/bin/activate`

     Do this **every time before you start** working.

7. Install security libraries on your environment

     `$ pip install -r requirements.txt`

     If --user error, run `$ sudo su` before installing but `$ exit` afterwards.

8. Install Trivy on Debian/Ubuntu/Kali
     Check [Trivy Releases](https://github.com/aquasecurity/trivy/releases) and replace the version number to install the latest one

     `$ wget https://github.com/aquasecurity/trivy/releases/download/v0.16.0/trivy_0.16.0_Linux-64bit.deb`

     and then run:

     `$ sudo dpkg -i trivy_0.16.0_Linux-64bit.deb`

9. Housecleaning
  - Make a today's date variable and folder to keep reports organized

     `$ now=`date +"%Y-%m-%d"` && mkdir ~/research/$now`

  - Make folders for each library you will be using (e.g. Bandit, Safety, Trivy, etc) inside of today's date folder

     `$ mkdir ~/research/$now/bandit && mkdir ~/research/$now/safety && mkdir ~/research/$now/trivy`

10. Clone the target repo (e.g PySyft) and jump to [Usage: Kali](#kali) to test with this setup.

### Parrot Security on Oracle VirtualBox
This setup is to test the PyGrid runtime and hunt for remote code execution vulnerabilities

1. Download and install [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)

2. Download [Parrot Security OVA](https://www.parrotsec.org/download/) virtual machine image and load it on VirtualBox

3. Inside your Parrot OS, [Install Docker Engine on Debian
](https://docs.docker.com/engine/install/debian/) with **one important change**. Add the repository by running:

    `$ sudo nano /etc/apt/sources.list`

    And appending:

    `deb [arch=amd64] https://download.docker.com/linux/debian buster stable`

4. Clone [PyGrid](https://github.com/OpenMined/PyGrid) and jump to [Usage: Parrot](#parrot) to test with this setup.


## Usage
### Kali
On Kali, to research the target repo using Bandit, first change directory into the target repo (e.g PySyft). *Replace **[REPO]** with the applicable repository name.*

`$ cd ~/research/[REPO]`

Better yet, you can `$ export REPO=PyGrid` and replace [REPO] with $REPO everywhere else. This notation is used from here on out.

#### Bandit
To run a [bandit](https://pypi.org/project/bandit/) generalized check and write a report log:

`$ bandit -r . >~/research/$now/bandit/$REPO-report.log`

And a medium to high severity, high confidence one:

`$ bandit -r -iii -ll . >~/research/$now/bandit/$REPO-iii-ll-report.log`

#### Safety
You don't need to be in the same directory to use Safety, but make sure you activated your virtual environment before running it. To run a safety check of your virtual environment and save the report, execute the following:

`$ safety check>~/research/$now/safety/$REPO-safety-check.log`

#### Trivy
You can use [Trivy](https://github.com/aquasecurity/trivy) to research Docker containers. For example, to check the PyGrid network development image, run:

`$ trivy image openmined/grid-network:development>~/research/$now/trivy/$REPO-network-report.log`

**Do not** use the *latest* tag as it is a known issue with Trivy.

### Parrot
Using your terminal, cd into your cloned PyGrid repo and run

`$ docker-compose up`

Once PyGrid is up and running on Docker, click the link on your terminal to your Jupyter Notebook instance (e.g. localhost) and look for vulnerabilities.


## Contributing
Pull requests are appreciated and encouraged. [Raise an issue](https://github.com/socd06/syft-hacks/issues/new)

## License
[MIT License](https://github.com/socd06/syft-hacks/blob/main/LICENSE)
