# syft-hacks
Security research on OpenMined repositories

# Getting started / how to replicate
1. Download and install [VMWare Workstation](http://www.vmware.com/go/tryworkstation-win)
2. Activate [VMWare Workstation](http://www.vmware.com/go/tryworkstation-win) with an [Open-Source license](https://my.vmware.com/en/web/vmware/downloads/details?downloadGroup=WKST-1610-OSS&productId=1038)
3. Download a compatible [Kali Linux VMWare image](https://www.offensive-security.com/kali-linux-vm-vmware-virtualbox-image-download/) and **load it on VMWare**
4. Inside your Kali Linux, [Make Python3 the default Python](https://thequickblog.com/how-to-change-default-version-of-python-as-python3/)
5. Install [virtualenv](https://pypi.org/project/virtualenv/) to avoid [Anaconda](https://www.anaconda.com/) and messing with Kali's Python settings.
6. Make a virtual environment with [virtualenv](https://pypi.org/project/virtualenv/) and activate it with the `source` command, mine looks like this:

     `source /home/kali/syft_venv/sec_audit/bin/activate`

     Do this **every time before you start** working.

7. Install security libraries on your environment

`pip install -r requirements.txt`

If --user error, run `sudo su` before installing but `exit` afterwards.

8. Clone the target repo (e.g PySyft)

9. Housecleaning
  - Make a today's date variable and folder to keep reports organized

  `now=`date +"%Y-%m-%d"` && mkdir ~/research/$now`

  - Make folders for each library you will be using (e.g. Bandit, Safety, etc) inside of today's date folder

  `mkdir ~/research/$now/bandit && mkdir ~/research/$now/safety`

# Attacking/Researching the target repo

cd into the target repo (e.g PySyft). Replace [REPO] with the applicable repository name.

`cd ~/research/[REPO]`

Better yet, for example, you can `export REPO=PyGrid` and replace [REPO] with $REPO everywhere else.

# Bandit
To run a [bandit](https://pypi.org/project/bandit/) generalized check and write a report log:

`bandit -r . >~/research/$now/bandit/[REPO]-report.log`

And a medium to high severity, high confidence one:

`bandit -r -iii -ll . >~/research/$now/bandit/[REPO]-iii-ll-report.log`

# Safety
To run a safety check and save the report:

`safety check>~/research/$now/safety/[REPO]-safety-check.log`
