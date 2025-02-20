# Automate-Infrastructure-with-IaC-using-Terraform
We have built AWS infrastructure for 2 websites manually, it is time to automate the process using Terraform.

Let us start building the same set up with the power of Infrastructure as Code (IaC)

![image](https://github.com/user-attachments/assets/fac6977f-7b36-45af-a702-3c408b3d95f5)


## Prerequisites before you begin writing Terraform code

**Step 1.** 
You must have completed Terraform course to understand the core concept of the idea of IaC

**Step 2**
Create an IAM user, name it `terraform` (ensure that the user has only programatic access to your AWS account) and grant this user AdministratorAccess permissions.
Alright! Since you have an overview, we’ll proceed while filling in any gaps along the way.  
Terraform needs permissions to interact with AWS resources, so we’ll create an **IAM user** with programmatic access.  
1. **Log in to AWS Console** → Go to **IAM (Identity & Access Management)**.  

2. **Create a New User**:  
   - **User name:** `terraform`  
   - **Access type:** **Programmatic access** (checkbox should be selected).  

![image](https://github.com/user-attachments/assets/75e20465-124b-433f-a4ea-062579986f4d)


3. **Attach Permissions:**  
   - Select **“Attach existing policies directly”**.  
   - Search for and attach **`AdministratorAccess`** policy (for learning purposes).

![image](https://github.com/user-attachments/assets/9acd9253-fd69-44db-b7be-7d3c01250570)
 
4. **Create the User** and **download the credentials file** or **copy**:  
   - **Access Key ID**  
   - **Secret Access Key**  
![image](https://github.com/user-attachments/assets/98b29f7d-bdd2-4ff3-bea8-f953d21fef15)


**Step 3**
Configure programmatic access from your workstation to connect to AWS using the access keys copied above and a Python SDK (boto3). You must have Python 3.6 or higher on your workstation.

Got it. Here’s **Step 3 from the beginning**, as if you haven't done anything yet.  

---

# **Step 3: Configure AWS CLI for Programmatic Access**  

**3.1 Install AWS CLI on Windows**  
Since we need AWS CLI to configure programmatic access, let's install it first.  

1️⃣ Open **Git Bash** (or PowerShell)  
2️⃣ Run this command to download and install AWS CLI:  
   ```bash
   msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
   ```
3️⃣ Follow the installation prompts and complete the setup.  

![image](https://github.com/user-attachments/assets/df238623-9bef-4a8a-891b-cc08038697ca)

---

**3.2 Verify AWS CLI Installation**  
After installation, check if AWS CLI is installed correctly:  
```bash
aws --version
```  
✅ If successful, you should see output like this:  
```
aws-cli/2.x.x Python/x.x.x Windows/x.x.x exe/MSI
```
![image](https://github.com/user-attachments/assets/b3a8b813-20d9-4e89-9164-ca77acb42b06)

 
❌ If you see **"command not found"**, restart your terminal and try again.  

---

### **3.3 Configure AWS CLI with Access Keys**  
Now, let's configure AWS CLI with your **IAM user’s access keys**.  

1️⃣ Run the following command:  
   ```bash
   aws configure
   ```  
2️⃣ When prompted, enter:  
   - **AWS Access Key ID:** _(Paste the access key you copied from IAM)_  
   - **AWS Secret Access Key:** _(Paste the secret key)_  
   - **Default region name:** _(e.g., `us-east-1`, `eu-west-1`, or your preferred region)_  
   - **Default output format:** _(Press Enter to use `json` or type `table`/`text`)_  

---

### **3.4 Test AWS CLI Connection**  
Run this command to verify that AWS CLI can access your AWS account:  
```bash
aws s3 ls
```  
✅ If everything is configured correctly, you should see a list of your S3 buckets (or an empty response if no buckets exist).  

![image](https://github.com/user-attachments/assets/d7b2d692-f91b-42c7-a29d-d535d9aeab6a)

---

#### **3.4 Create an S3 Bucket**  
S3 bucket names must be **globally unique**, so choose a unique name.

```bash
aws s3 mb s3://your-unique-bucket-name --region us-east-1
```
For example:  
```bash
aws s3 mb s3://captainfresh090-dev-terraform-bucket --region us-east-1
```
If your region is different, replace `us-east-1` with the correct region.

---

#### **Step 4: Verify Bucket Creation**  
To check if the bucket was successfully created, list all your S3 buckets:  
```bash
aws s3 ls
```
You should see an output like this:  
```
2025-02-18 12:45:30 captainfresh090-dev-terraform-bucket
```

![image](https://github.com/user-attachments/assets/873fb0ed-2adc-4cf8-bf3c-59b9aebc3204)

---

## The secrets of writing quality Terraform code

The secret recipe of a successful Terraform projects consists of:
* Your understanding of your goal (desired AWS infrastructure end state)
* Your knowledge of the IaC technology used (in this case – Terraform)
* Your ability to effectively use up to date Terraform documentation here

  
As you go along completing this project, you will get familiar with Terraform-specific terminology, such as:

* Attribute
* Resource
* Interpolations
* Argument
* Providers
* Provisioners
* Input Variables
* Output Variables
* Module
* Data Source
* Local Values
* Backend


Make sure you understand them and know when to use each of them.

Another concept you must know is data type. This is a general programing concept, it refers to how data represented in a programming language and defines how a compiler or interpreter can use the data. Common data types are:

* Integer
* Float
* String
* Boolean, etc.

## Best practices
* Ensure that every resource is tagged using multiple key-value pairs. You will see this in action as we go along.
* Try to write reusable code, avoid hard coding values wherever possible. (For learning purpose, we will start by hard coding, but gradually refactor our work to follow best practices).


Great! 🎉 Now that we've verified programmatic access, we can move on to the next step.  

---
## Install Terraform (If You Haven't Already)
Run this command to verify Terraform is installed:
```bash
terraform -version
```
If it’s not installed, download and install Terraform from the [official Terraform website](https://developer.hashicorp.com/terraform/downloads).

### **🔹 Installing Terraform on Windows**
Since you haven’t installed Terraform yet, follow these steps carefully.

---

### **Step 1: Download Terraform**
1️⃣ Go to the [official Terraform website](https://developer.hashicorp.com/terraform/downloads).  
2️⃣ Scroll down and select **Windows** as your OS.  
3️⃣ Download the **.zip** file for the latest version.

---

### **Step 2: Extract & Move Terraform**
1️⃣ Extract the **.zip** file to a location of your choice (e.g., `C:\terraform`).  
2️⃣ Copy the `terraform.exe` file inside the extracted folder.  
3️⃣ Move `terraform.exe` to a directory that’s in your **system PATH**, or add its location to PATH.

---

### **Step 3: Add Terraform to System PATH**
1️⃣ Open **File Explorer**, right-click on **This PC**, and select **Properties**.  
2️⃣ Click on **Advanced System Settings** → **Environment Variables**.  
3️⃣ Under **System Variables**, find and select **Path**, then click **Edit**.  
4️⃣ Click **New**, and paste the path where `terraform.exe` is located (e.g., `C:\terraform`).  
5️⃣ Click **OK** to save changes.

---

### **Step 4: Verify Terraform Installation**
1️⃣ Open **Command Prompt (cmd)** or **PowerShell**.  
2️⃣ Run the following command:  
   ```bash
   terraform -version
   ```
3️⃣ You should see output similar to:  
   ```
   Terraform v1.xx.x
   ```

![image](https://github.com/user-attachments/assets/0a25a569-77a7-48c0-9751-e1805164a8bc)

---


## CREATING VPC | SUBNETS | SECURITY GROUPS

### **Step 1: Create the Terraform Directory & File**  
1️⃣ Open **VS Code**.  
2️⃣ Create a new folder named **`PBL`** (inside `C:\Users\PC\` or any preferred location).  
3️⃣ Inside the `PBL` folder, create a new file named **`main.tf`**.  

Great! Now, let's move to the next step.  

---

### **Step 2: Define the Provider & VPC in `main.tf`**  
1️⃣ Open `main.tf` in VS Code.  
2️⃣ Copy and paste the following Terraform code into the file:  

```
# Define AWS Provider
provider "aws" {
  region = "eu-central-1"  # Change this to your preferred AWS region
}

# Create a VPC
resource "aws_vpc" "main" {
  cidr_block                     = "172.16.0.0/16"
  enable_dns_support             = true
  enable_dns_hostnames           = true
  enable_classiclink             = false
  enable_classiclink_dns_support = false
}
```

3️⃣ **Save the file.**  

![image](https://github.com/user-attachments/assets/8f919834-0f4f-4170-a634-14509b5a46d6)

---

### **Step 3: Initialize Terraform**  
1️⃣ Open **Terminal** in VS Code (press `Ctrl + ~` to open it).  

2️⃣ Navigate to your **PBL** folder (if you’re not already there):  
```bash
cd C:\Users\PC\PBL
```

3️⃣ Run the **terraform init** command to initialize the directory:  
```bash
terraform init
```

---

### ✅ **Expected Output:**  
You should see something like this:  
```
Initializing the backend...
Initializing provider plugins...
- Finding latest version of hashicorp/aws...
- Installing hashicorp/aws vX.X.X...
- Installed hashicorp/aws vX.X.X...
Terraform has been successfully initialized!
```
![image](https://github.com/user-attachments/assets/25d78519-c330-4140-8ada-edf8b32559a5)

---

### **Step 4: Run Terraform Plan**  
This command will analyze the `main.tf` file and show you what Terraform is about to create.

1. Run the following command in your **PBL** directory:  
```bash
terraform plan
```

2. Terraform will display a list of resources that will be created. It should include something like:
```
Terraform will perform the following actions:
  # aws_vpc.main will be created
  + resource "aws_vpc" "main" {
      + cidr_block = "172.16.0.0/16"
      + enable_dns_support = true
      + enable_dns_hostnames = true
      ...
    }
Plan: 1 to add, 0 to change, 0 to destroy.
```
This means Terraform has **recognized** your VPC configuration and is ready to deploy.

![image](https://github.com/user-attachments/assets/5b87aa72-f25a-4716-b781-d912626ab6d5)

![image](https://github.com/user-attachments/assets/ad0fd0b5-f80e-4463-84fc-481d9a647297)


---

Now that `terraform plan` is working fine, it's time to **apply** the changes and create the VPC.  

### **Next Step: Apply Terraform Configuration**
Run:  
```bash
terraform apply
```
Terraform will show you a summary of what it will create and ask for confirmation. Type **`yes`** when prompted.

---

- Terraform will create the **VPC** as defined in `main.tf`.
- A new file, `terraform.tfstate`, will be generated to track your infrastructure state.

![image](https://github.com/user-attachments/assets/48d7e9d1-72e3-4614-a767-805e4de4c5c7)































