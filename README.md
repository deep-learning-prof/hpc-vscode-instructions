<div align="center">
  <img src="https://i.imgur.com/0Mx7Zpd.png">
</div>

Wichita State University's high-performance computing (HPC) cluster is here to help researchers, instructors and staff who have compute-intensive jobs to process. The research computing cluster is named “BeoShock” after “Beowulf” cluster and Shockers.

Beowulf is a type of computing cluster. The cluster has four GPUs (Graphics Processing Units) and 720 CPU (Central Processing Unit) “cores.” As a comparison, most end-user computing devices have one CPU having between one and eight total “cores.”

The cluster is available to all WSU constituents, and those outside of WSU who are KBOR constituents. 

## Table of Content
1. [Getting Access to the HPC](#getting-access-to-the-hpc)
2. [Installing VPN](#installing-vpn)
3. [Installing VSCode](#installing-vscode)
4. [Connecting to Beoshock](#connecting-to-beoshock)
5. [Developing code on Beoshock](#developing-code-on-beoshock)
6. [Uploading your code to Beoshock](#uploading-your-code-to-beoshock)
7. [Running Python code on Beoshock](#running-python-code-on-beoshock)
8. [Monitoring your code](#monitoring-your-code)
9. [Helpful Tips](#helpful-tips)


## Getting access to the HPC
- The HPC is open to WSU students without additional costs. However, you must request access.
- Follow the [link](https://www.wichita.edu/services/hpc/index.php) to request access to the HPC.
- Click on the button that says "New User Request".
- Fill out the form. If you have questions on how to fill the form, ask your instructor.
- Your request should be approved within 1-2 days.

## Installing VPN
- You must be on-campus or connected to the VPN to access the HPC.
- You can download the VPN by following instructions on this [link](https://www.wichita.edu/services/its/ITSApplicationsTraining/VPNWIndows.php).
- While the above link doesn't say so, you can get GlobalProtect on Linux, for example on [AUR](https://aur.archlinux.org/packages/globalprotect-bin) or Fedora [Copr](https://copr.fedorainfracloud.org/coprs/yuezk/globalprotect-openconnect/).
- There are other ways to grab it on Linux, but we won't cover it on this guide.

## Installing VSCode
- Install VSCode on your computer. VSCodium may work if the SSH extension can be installed.
- Follow the [link](https://code.visualstudio.com/Download) to download the appropriate file for your OS. You may also follow the following [link](https://code.visualstudio.com/docs/setup/linux) to set it up directly with your package manager.
### Installing the VSCode SSH Plugin
- The HPC can be accessed using Secure Shell (ssh). However, directly using a terminal to access the HPC can be tedious and time consuming. You will need to manually reconnect every time the session times out or there is an issue with you Internet connection. Also, you may prefer to use a GUI text editor instead of a terminal-based editor.
- Instead, we will use the Remote-SSH plugin provided by Microsoft. Go to the extensions menu on vs-code. You can find this menu by clicking on the icon that has three squares together and a fourth one slighlty away from the rest.
- Type "ssh" in the extensions search bar, select Remote-SSH, and click install.

## Connecting to Beoshock
- Click on the green button on the bottom-left corner. The button icon has a greater than (>) and a less than (<) symbols.
- After clicking the button, you should see a drop down menu on the top.
- Select "Connect to host. . . ".
- Enter your WSU ID and the domain name of the HPC server as follows: "mywsuid@hpc-login.wichita.edu" without the quotes.
- Enter your password, and this will open a new VSCode window. This acts as your interface to the HPC.
- If this is the first time that you are logging in to the HPC with VScode, you may need to wait a 2-5 minutes while the server program is installed on the HPC.

## Developing code on Beoshock
- If you are starting from scratch, you can write your own code directly on Beoshock.
- To create a new file, click on the new file icon. The new file icon is under the Explorer button on the navigation bar. The icon is two pieces of paper on top of each other.
- Hover your mouse over the name of the folder on the navigation column. The new file icon should appear. Click it.
- Write your code.

## Uploading your code to Beoshock
- The easiest way to get your code on Beoshok is using git and git hosting provider of your choice such as GitHub. You can simply get your code using the git clone command.
- To run the command, press Ctrl+~ . This should brig up a terminal window.
- In the terminal window, make sure you are in your home directory by typing `cd .`.
- Type git clone URL-to-Repo , where URL-to-Repo is the URL of the GitHub (or provider of choice) repository you want to clone.
- The git clone command will create a new directory with the name of the repository.
- Enter the repository folder by typing cd name-of-folder, where name-of-folder is the name of the newly created folder.
- Alternatively, you can drag and drop the directory from your machine to files tab in VSCode, but this won't keep the two directories in sync.

## Running Python code on Beoshock
- When you login to the HPC, you login to a special computer inside the HPC called the "headnode".
- The headnode serves as a gatekeeper to the vast computing resources of the HPC.
- That is, users are not allowed to directly run their code.
- Instead, users need to request the HPC to run their code with certain amounts of computing resources.
- Once there are enough resources for the code, the HPC will run it and write the results into the users’ folder.
- So how do you request that the HPC runs your code?
- To this end, we use the sbatch command. The format of the command is `sbatch nameOfFileWithCode [options]`, where options indicate the amount of resources you need for your code. 
- However, directly running that command on the terminal is error-prone and tedious.
- Instead, we recommend using a script. Ask your instructor for a sample script. Once you have the script, you can run it by typing `./nameOfScript nameOfFileWithCode`.
- You can edit the requested computing resources as needed.

## Monitoring your code
- Every time you run your code, the HPC will assign it a number.
- The output of your code will be written into a file named after your code number in the same folder from which you ran the script. Keep an eye on that folder to see the output of your program
- If the program crashes, the error message will also be written in this file.
- Use the command kstat to check if your program ran immediately or if is in the queue.
- You can stop a program with the command `scancel [program number]`.

## Helpful Tips
- You can turn off your computer or disconnect vs-code while the program is running. The results will be
  waiting when you come back.

## Sample run script. 
```
#!/bin/bash
 
#SBATCH --time=1:00:00
#SBATCH --mem-per-cpu=1G
#SBATCH --cpus-per-task=8
#SBATCH --ntasks=1
#SBATCH --nodes=1
#SBATCH --gres=gpu:1
 
echo "Reached before python lines"
module load Python/3.7.4-GCCcore-8.3.0
source /home/j237k972/python-env/dga-detection-env/bin/activate
python $1
echo "After python lines"
 
# To use this file on an Slurm cluster run: sbatch launch.sh <name of your python-script>
```