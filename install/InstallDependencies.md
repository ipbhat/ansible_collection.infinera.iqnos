# Install Python 


## Steps to install Python3 ans pip3

### Install using yum or apt

- Red Hat or CentOS

     ```bash
     sudo yum update
     sudo yum install python3

     sudo yum install python3-pip

     ```

- Ubuntu

   ```bash
   sudo apt-get update
   sudo apt-get install python3
   sudo apt-get install python3-pip

   or
   sudo dnf install python3
   sudo dnf install python3-pip
   ```
### Install from python.org

1. Navigate to https://www.python.org/ on the web browser.

2. Select the link to download and click Python version 3.6+ installer listed.

3. Run the installer and follow the installation steps.

## Install Python dependecies required by IQNOS collection modules 
The Ansible IQNOS collection modules require the following python packages to be installed which is under ```requirements.txt```

  * The following command will install the packages according to the configuration file ```requirements.txt```
  
      ```
      pip3 install -r requirements.txt
## Links for Reference
* [Python3](https://www.python.org/downloads/)
* [BeginnersGuide](https://wiki.python.org/moin/BeginnersGuide/Download)
