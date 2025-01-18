Here’s a comprehensive guide for setting up an EC2 instance, connecting to it from VS Code, setting up the development environment, and running a Jupyter notebook. This guide is designed to be as simple and streamlined as possible.

---

### **Step 1: Create an EC2 Instance**

1. **Login to AWS Console**:  
   Go to the [AWS Management Console](https://aws.amazon.com/console/) and log in.

2. **Navigate to EC2**:  
   In the AWS Console, search for **EC2** and open the EC2 Dashboard.

3. **Launch an Instance**:
   - Click on **Launch Instance**.
   - **Choose an Amazon Machine Image (AMI)**:  
     For simplicity, select **Amazon Linux 2 AMI** (free tier eligible).
   - **Choose an Instance Type**:  
     Select the instance type that fits your needs (for this guide, we will use `t2.micro`, which is free-tier eligible).
   - **Configure Instance Details**:  
     Leave the default settings as they are.
   - **Add Storage**:  
     The default 8 GB is sufficient for this setup.
   - **Tag the Instance** (Optional):  
     You can add tags if desired (e.g., Name: MyInstance).
   - **Configure Security Group**:  
     - Add a rule for SSH:  
       - Type: `SSH`  
       - Protocol: `TCP`  
       - Port Range: `22`  
       - Source: `My IP` (this will restrict SSH access to your IP address only).
   - **Review and Launch**:
     Review your settings and click **Launch**.
   - **Select Key Pair**:  
     If you don't have an existing key pair, create a new one. Make sure to **download the `.pem` file**.
     - Click **Launch Instances**.

4. **View Your Instance**:
   - After launching, go to the **Instances** section in EC2 Dashboard.
   - You should see your instance running. Note down the **Public IPv4 DNS** or **Public IP**.

---

### **Step 2: Configure VS Code to Connect to the EC2 Instance**

1. **Install VS Code**:  
   If you haven’t already, download and install [VS Code](https://code.visualstudio.com/).

2. **Install the Remote Development Extension**:  
   In VS Code, install the **Remote - SSH** extension:
   - Open VS Code.
   - Go to the Extensions view (`Ctrl+Shift+X`).
   - Search for **Remote - SSH** and install it.

3. **Configure SSH in VS Code**:
   - Open the **Command Palette** (`Ctrl+Shift+P`).
   - Search for **Remote-SSH: Connect to Host**.
   - Select **Configure SSH Hosts**.
   - If prompted, choose the **SSH config file** to edit.
   
   Add the following to your SSH config file (`~/.ssh/config`):

   ```bash
   Host my-ec2-instance
       HostName <your-ec2-public-ip>
       User ec2-user
       IdentityFile /path/to/your-key.pem
   ```

   Replace `<your-ec2-public-ip>` with the **Public IP** of your EC2 instance, and `/path/to/your-key.pem` with the path to your `.pem` key file.

4. **Connect to the EC2 Instance**:
   - Open the **Command Palette** again (`Ctrl+Shift+P`).
   - Search for **Remote-SSH: Connect to Host**.
   - Select **my-ec2-instance**.
   - VS Code will now connect to your EC2 instance. Once connected, you can open the terminal inside VS Code and work remotely.

---

### **Step 3: Set Up Development Environment on the EC2 Instance**

1. **Update the EC2 Instance**:
   Open the VS Code terminal (connected to the EC2 instance) and run the following commands:

   ```bash
   sudo yum update -y
   ```

2. **Install Python**:
   - Install Python 3 and `pip` (if not already installed):
   
   ```bash
   sudo yum install python3 -y
   ```

   - Verify Python 3 installation:

   ```bash
   python3 --version
   ```

3. **Install Git**:
   - Install Git using the following command:

   ```bash
   sudo yum install git -y
   ```

   - Verify Git installation:

   ```bash
   git --version
   ```

4. **Install Jupyter**:
   - Install Jupyter via pip:

   ```bash
   pip3 install jupyter
   ```

   - Verify installation:

   ```bash
   jupyter --version
   ```

---

### **Step 4: Clone the Repository**

1. **Clone Your Repository**:
   - In the VS Code terminal, navigate to the directory where you want to store your repository.
   
   ```bash
   git clone <your-repository-url>
   cd <your-repository-name>
   ```

---

### **Step 5: Install Required Libraries**

1. **Create and Activate a Virtual Environment**:
   - First, create a new virtual environment:

   ```bash
   python3 -m venv myenv
   ```

   - Activate the virtual environment:

   ```bash
   source myenv/bin/activate
   ```

2. **Install Required Libraries**:
   - If your repo has a `requirements.txt` file, install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

   Otherwise, manually install any libraries required for your notebook, such as `numpy`, `pandas`, etc.:

   ```bash
   pip install numpy pandas
   ```

---

### **Step 6: Run the Jupyter Notebook**

1. **Start the Jupyter Notebook**:
   - In the terminal, run the following command to start Jupyter:

   ```bash
   jupyter notebook --ip=0.0.0.0 --no-browser --port=8888
   ```

   - This command will start Jupyter Notebook and allow connections from all IP addresses on port `8888`.

2. **Access Jupyter Notebook**:
   - Open a browser on your local machine.
   - Go to `http://<your-ec2-public-ip>:8888/`.
   - You’ll be prompted for a token. Get the token from the terminal where you started Jupyter, which will be printed in the output.

---

### **Step 7: Stop the EC2 Instance**

1. **Stop the Instance**:
   - In VS Code, use the AWS Toolkit or EC2 Dashboard to stop your EC2 instance to avoid ongoing costs.
   
   You can also stop the instance via the AWS Console:
   - Go to **EC2 Dashboard** > **Instances** > Select your instance > Click **Instance State** > **Stop Instance**.

---

### **Step 8: Terminate the EC2 Instance and Resources**

1. **Terminate the Instance**:
   - In the **EC2 Dashboard**, select your instance, then click on **Actions** > **Instance State** > **Terminate**.

2. **Delete Any Other Resources**:
   If you created additional resources (like Elastic IPs, security groups, or volumes), make sure to delete them as well to avoid charges.

---

### **Conclusion**

This guide walks you through creating an EC2 instance, connecting it to VS Code, setting up a Python development environment, cloning a repo, installing dependencies, running a Jupyter notebook, and properly shutting down your resources.