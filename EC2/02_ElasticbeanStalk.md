Here’s a simple Python web application using **Flask**, a lightweight web framework, that you can deploy using AWS **Elastic Beanstalk**.

### **Prerequisites**

1. **AWS Elastic Beanstalk CLI**: You need to have the **Elastic Beanstalk Command Line Interface (EB CLI)** installed on your local machine.
2. **Python**: Ensure that **Python 3.x** is installed on your machine.
3. **Flask**: A simple Python web framework.

---

### **Step 1: Create a Simple Python Flask Application**

1. Create a directory for your project:
   ```bash
   mkdir flask-eb-app
   cd flask-eb-app
   ```

2. Inside this directory, create a file called `application.py`:

   ```python
   from flask import Flask

   # Create a Flask application instance
   application = Flask(__name__)

   @application.route('/')
   def hello_world():
       return "Hello, Elastic Beanstalk! This is a Flask app deployed on AWS."

   if __name__ == "__main__":
       # Running the Flask application
       application.run(host='0.0.0.0', port=8080)
   ```

   - This simple Flask app listens on the root URL (`/`) and returns a message when accessed.

3. Create a file named `requirements.txt` to define the Python dependencies:
   ```txt
   Flask==2.0.3
   ```

   This file tells Elastic Beanstalk which Python packages need to be installed for the application to run.

---

### **Step 2: Install the Dependencies Locally**

You can test the application locally before deploying it to Elastic Beanstalk.

1. Create a virtual environment and activate it:
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # For MacOS/Linux
   # On Windows: venv\Scripts\activate
   ```

2. Install the dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Run the Flask app locally:
   ```bash
   python application.py
   ```

   Open your browser and go to `http://localhost:8080`. You should see the message: **"Hello, Elastic Beanstalk! This is a Flask app deployed on AWS."**

---

### **Step 3: Deploy the Python Application Using Elastic Beanstalk**

1. **Initialize the Elastic Beanstalk Application**:

   In your project directory, initialize the Elastic Beanstalk environment:
   ```bash
   eb init
   ```

   - **Select Region**: Choose the AWS region where you want to deploy the application.
   - **Application Name**: Enter the application name (e.g., `flask-eb-app`).
   - **Platform**: Select `Python`.

2. **Create an Elastic Beanstalk Environment**:

   Run the following command to create an environment and deploy the application:
   ```bash
   eb create flask-env
   ```

   - This command will create an environment called `flask-env`, set up the infrastructure (EC2 instances, load balancers, etc.), and deploy the Flask application.

3. **Access the Deployed Application**:

   Once the environment is created, Elastic Beanstalk will provide a URL (e.g., `http://flask-env.eba-xyz.amazonaws.com`). You can access this URL in your browser to see your Flask application live on AWS Elastic Beanstalk.

---

### **Step 4: Update the Application**

If you make changes to your Python application, you can deploy the updated version using:
```bash
eb deploy
```

This command will push the new version of your app to Elastic Beanstalk.

---

### **Step 5: Monitoring and Managing the Environment**

You can monitor your environment’s health and manage other aspects using the Elastic Beanstalk console or through the EB CLI:
- To check the environment status: `eb status`
- To view logs: `eb logs`
- To open the Elastic Beanstalk environment in the browser: `eb open`

---

### **Optional: Cleanup**

When you're done, you can terminate the Elastic Beanstalk environment to avoid costs:
```bash
eb terminate flask-env
```

---

### **Summary**

This tutorial walks through deploying a simple **Flask** application on **AWS Elastic Beanstalk**. Elastic Beanstalk handles all the infrastructure setup, including the creation of EC2 instances, load balancers, and scaling configurations, allowing you to focus purely on the application. 
