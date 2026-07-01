# 🏥 Welcome to the New Insurance Claims Processing Agent

Welcome to an AI-powered insurance claims processing system using **LangGraph**, **RAG**, and **GPT-4o-mini** to automate claim validation, policy retrieval, and decision-making.

##  Here are the Features

- ✅ **FEATURE: Automated Claims Processing**: Parse, provide validation for, and adjudicate insurance claims
- 🔍 **FEATURE: RAG-based Policy Retrieval**: Intelligent retrieval from policy documents through the use of ChromaDB
- 📊 **FEATURE: Interactive UI**: Streamlit interface for manual entry or JSON upload
- 🤖 **FEATURE: LangGraph Workflow**: Structured multi-step agent workflow

🐳 **FEATURE: Docker Support**: Easy deployment with Docker

- 📝 **FEATURE: Detailed Logging**: Complete audit trail of all agent decisions
- 

## 📋 System Requirements

- Python 3.11+
- OpenAI API key
- (optional) Docker  (for containerized deployment)

## 🛠️ Running Streamlit and Installation Methods

## Running the Application

1.  **Start the Streamlit server**:
    ```bash
    streamlit run app/main.py
    ```

2.  **Access the Web Interface**:
    *   Open your web browser and go to: `http://localhost:8501`


### I. Local Setup in your instance

<br>
1. **Clone the repository**
    ```bash
    git clone https://github.com/rfoldgh-port/insurance-agent-new.git
    cd insurance-agent
    ```

2.  **Create a virtual environment** (recommended):
    ```bash
    python -m venv virtual_environment
    
    # Windows
    virtual_environment\Scripts\activate
    
    # Linux/Mac
    source virtual_environment/bin/activate
    ```

3.  **Install dependencies**:
    ```bash
    pip install -r requirements.txt
    ```

4.  **Configure Environment Variables**:
    *   Find the `.env` file in the project root.
    *   Add your OpenAI API Key & OpenAI Base URL:
        ```env
        OPENAI_API_KEY="xxxxxxxxxx"      #  Add your OpenAI API Key
        OPENAI_BASE_URL="https....../v1"    #  Add your OpenAI Base URL


### II. Deploy on AWS EC2 Instance (Clone GitHub  →  Build Docker Image  →  Docker Run)

## Steps for Building the Docker Image on your AWS EC2 Instance

## Launch an EC2 Instance

1. **Log into AWS Console** - Navigate to the EC2 Dashboard from the Services menu under Compute

2. **Click "Launch Instance"** - Enter a name for your instance (e.g., "my-docker-server")

3. **Choose an AMI** - Select Ubuntu Server 20.04/22.04 LTS

4. **Select Instance Type**- Choose t2.micro or t3.micro for Free Tier eligibility, or select based on your resource needs
​
5. **Configure Key Pair** - Create a new SSH key pair, download the .pem file and save it securely
​
6. **Configure Security Group** - Allow SSH, HTTP, and HTTPS access (Check all 3 options under the Network/Security Group section).This configuration ensures that the Docker container can expose web application ports (such as 80 and 443) externally.
<br>**Note:** (Preferred but Optional for this demonstration - Restrict SSH to your IP address for security)
​
8. **Launch Instance** - Review your configuration and click "Launch Instance"

## Connect to Your EC2 Instance

1. **SSH into your instance using:**
```bash
ssh -i /path/to/your-key.pem ubuntu@your-instance-public-ip
```

2.  **Install Docker on EC2**:
```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
sudo apt install git -y
```

## Clone Your GitHub Repository and Build Docker Image

1. Install Git (if not already installed):
```bash
sudo apt install git -y  # Ubuntu
```

2. Clone your repository:
```bash
git clone https://github.com/rfoldgh-port/insurance-agent-new.git
cd insurance-agent
```

3. Add your credentials in the .env file
```bash
nano .env

OPENAI_API_KEY="xxxxxxxxxxxxx"      # Modify this line and add your OpenAI API Key
OPENAI_BASE_URL="https....../v1"    #  Modify this line and add your OpenAI Base URL

CTRL + X
SHIFT + Y
Enter
```

4. Build your Docker image:
```bash
sudo docker build -t insurance-agent:v1 .
```

5. Check your Docker image:
```bash
sudo docker images insurance-agent:v1
```
 
6. Run your Docker image:
```bash
sudo docker run -d -p 80:8501 insurance-agent:v1
```

7. Visit your Public IP in any browser


### III. Use Pre-Built Docker Image from Docker Hub (Docker Image Pull  →  Docker Run)

## Steps for pulling the pre-build Docker Image from Docker Hub in your Instance

1.  **Install Docker on Instance**:
```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
sudo apt install git -y
```

2. **Pull the Docker Image from DockerHUB**
```bash
sudo docker pull rfoldgh-port/insurance-agent:v1
```

3. **Verify the Docker Image**
```bash
sudo docker images
```

4. **Pass the OpenAI API Key and the Base URL and Run the Docker Image**
```bash
sudo docker run -d \
  --name insurance-agent \
  --restart unless-stopped \
  -e OPENAI_API_KEY=" < YOUR OPENAI API KEY > " \
  -e OPENAI_BASE_URL=" < YOUR OPENAI BASE URL > " \
  -p 80:8501 \
  gr8learning/insurance-agent:v1

```



        ```





## Usage

1.  Enter your insurance claim details manually or upload the claim details in the JSON format
2.  Click **"Process Claim"**, to check if the claim is to be Approved or Rejected.
3.  Monitor the **Logs** of the agent.
