---
type: learning
date: 2026-06-17
status: In Progress
topics: ["Skills and Tools"]
tags: [type/learning, topic/skills-and-tools]
notion: https://app.notion.com/p/383d68da32f0809fa826c47c62f920db
created: 2026-06-18
updated: 2026-06-23
---
# How to Start Compute Canada

> [!summary] In short
> Step-by-step guide: HPC access, two-factor auth, SSH, Fir vs Nibi, submitting jobs.

**Topics:** [[Skills and Tools]]  
**Notion category:** [[Notion Category - Documentation]]  
**Original:** [Notion page](https://app.notion.com/p/383d68da32f0809fa826c47c62f920db)

---

### Activate HPC Server Access
Log in to [Compute Canad](https://ccdb.alliancecan.ca/security/login)a (https://ccdb.alliancecan.ca/security/login), then go to Resources/Access Systems.

![[how-to-start-compute-canada-1.webp]]

In the HPC system, you send a request for each access that you want. Currently, we only have access to HPC systems. For the HPC data center that you need, click “I request access” and they will grant the access immediately without any waiting.
We usually use Fir because it is at SFU, but it sometimes faces challenges. I have tested Nibi, which is more reliable, probably because there are fewer users.

### Two-step auth activation
Go to My Account/Multifactor Authentication 
Activate your two-factor auth. You need the Duo app on your phone (Google Authenticator does not work).

![[how-to-start-compute-canada-2.webp]]

### Open the cmd or bash
Use one of these depending on the HPC server you selected (use your Compute Canada username):
ssh [username@nibi.alliancecan.ca](mailto:username@nibi.alliancecan.ca)
  ssh [username@fir.alliancecan.ca](mailto:username@fir.alliancecan.ca)
After that, press 1 and Enter (it is obvious, but sometimes it does not register).
You may need to enter it multiple times. Then check ls or another Linux-related command; if it succeeds, let’s continue in VS Code.

Use mkdir in the scratch folder with the name of your project, for example Test.

### Install remote SSH in VS Code

![[how-to-start-compute-canada-3.webp]]

Then open a terminal from Terminal/New Terminal (this may not be required, so you can skip this new terminal).
Then press Ctrl+Shift+P, type Remote, and select Remote-SSH: Add New SSH Host.

![[how-to-start-compute-canada-4.webp]]

Then follow the steps to set it up.

After you set up your connection the first time, you only need to use Connect to Host after that.

When the window opens, type the password in the box, then click Details (bottom right) to complete the two-step verification. The password is your Compute Canada password.

![[how-to-start-compute-canada-5.webp]]

After logging in, open a folder.

![[how-to-start-compute-canada-6.webp]]

Go and find the Test folder you already made. If you missed that step, no problem; just open the scratch folder. VS Code will ask you for the password and two-step auth again each time you use Open Folder.

### Your first job submission
- Create test.py

```python
import random
import os

# Generate random data
random_data = [random.randint(0, 100) for _ in range(10)]  # List of 10 random numbers between 0 and 100

# Create a random file name
random_file_name = f"random_data_{random.randint(1000, 9999)}.txt"

# Save the random data to the file
with open(random_file_name, "w") as file:
    for number in random_data:
        file.write(f"{number}\n")

```
- Create run_alliance.bash

```shell
#!/bin/bash

# The SBATCH directives must appear before any executable line in this script.

#SBATCH --time=00:15:00      # this is 5 minutes (always start with this to test whether your script runs properly before requesting for a longer job time). Time format: D-H:M:S (e.g., 5-0:0:0 is 5 days)
#SBATCH --account=def-ester # DO NOT MODIFY THIS LINE
#SBATCH --mem=24G           # total CPU memory 
#SBATCH --nodes=1           # number of nodes requested
#SBATCH --ntasks-per-node=8 # CPU cores per node.
#SBATCH --cpus-per-task=1   # number of cores per task (if your code does not support multi-processing, you can just request for 1)
#SBATCH --gpus=h100:1
##SBATCH --gres=gpu:p100l:1 # one 16G VRAM P100 GPU per node (uncomment this and comment the line above if you want to use P100)
#SBATCH --mail-user=yourEmail@sfu.ca # change this to your email
#SBATCH --mail-type=BEGIN
#SBATCH --mail-type=END
#SBATCH --mail-type=FAIL
#SBATCH --mail-type=REQUEUE
#SBATCH --mail-type=ALL
#SBATCH --output=output%A.out

source .venv/bin/activate
module load StdEnv/2020
module load python/3.8.10

date

python3 test.py
deactivate

# print completion time
date

```
!!! Be careful: you may need to make some changes, such as editing your email.

Then run these commands to create the .venv:

module load python
  
module load python scipy-stack
  
virtualenv --no-download ~/my_env
  

Then you can activate it and pip install other packages if needed, or just run this command to start the environment for testing.
This command will submit the code to the server.

sbatch run_alliance.bash
