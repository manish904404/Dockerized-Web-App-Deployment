✅ 1. Prerequisites
Before deployment, ensure:

You have an IBM Cloud account: https://cloud.ibm.com/registration

IBM Cloud CLI installed: https://cloud.ibm.com/docs/cli

Docker is installed locally

IBM Cloud Container Registry plugin installed:

bash
Copy
Edit
ibmcloud plugin install container-registry
ibmcloud plugin install kubernetes-service

🧱 2. Project Structure Overview
The project has:

bash
Copy
Edit
/frontend      -> React UI
/backend       -> Node.js API with OpenWeatherMap integration
/docker-compose.yml
🛠️ 3. Make Required Edits
🔐 Add API Key
In /backend/keys.js:

js
Copy
Edit
const appid = "YOUR_API_KEY";
module.exports = appid;
🐳 4. Build and Push Docker Images to IBM Cloud Container Registry
Step 1: Log in to IBM Cloud CLI
bash
Copy
Edit
ibmcloud login --sso
Step 2: Set target region and resource group
bash
Copy
Edit
ibmcloud target -r us-south
ibmcloud target -g Default
Step 3: Log in to IBM Cloud Container Registry
bash
Copy
Edit
ibmcloud cr login
Step 4: Tag and push images
Update your docker-compose.yml to use IBM CR image URLs.

Let's assume:

Your registry namespace: weatherapp

Region: us.icr.io

Update docker-compose.yml services like this:

yaml
Copy
Edit
frontend:
  image: us.icr.io/weatherapp/weatherapp-frontend
  ...

backend:
  image: us.icr.io/weatherapp/weatherapp-backend
  ...
Then build and push images:

bash
Copy
Edit
docker build -t us.icr.io/weatherapp/weatherapp-frontend ./frontend
docker build -t us.icr.io/weatherapp/weatherapp-backend ./backend

docker push us.icr.io/weatherapp/weatherapp-frontend
docker push us.icr.io/weatherapp/weatherapp-backend
☁️ 5. Deploy on IBM Cloud Kubernetes or Code Engine
Option 1: IBM Cloud Code Engine (Recommended for simplicity)
Go to: https://cloud.ibm.com/codeengine

Create a project.

Create a New App → From Container Image

Use:

Image: us.icr.io/weatherapp/weatherapp-frontend

Port: 80 or 3000

Do the same for backend on port 9000

Use environment variables if needed to inject your API key instead of hardcoding in keys.js.

IBM Code Engine auto-scales, free tier available.

