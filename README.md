![image](https://github.com/user-attachments/assets/032b329e-91dd-4efa-9c36-949334526439)# Automate-Infrastructure-with-IaC-using-Terraform
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
















