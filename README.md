Streamlit Menu App - Roadmap

### **1. Local Development (All-in-One Monolithic App)**
1. **Set Up Your Environment:**
   - Install Python, Docker, and Docker Compose on your local machine.
   - Install necessary packages for Streamlit, FastAPI, SQLAlchemy, etc.
   - Set up a basic file structure for your project.

2. **Frontend:**
   - Develop the **customer and restaurant frontend** with Streamlit.
   - Create two distinct views in the same Streamlit app using tabs: one for the customer view and one for the restaurant view.
   - **Customer View:** Display menu items (with image, description, price) and allow customers to add items to an order.
   - **Restaurant View:** Display the orders placed by customers in real-time. Allow restaurant staff to edit the menu items.

3. **Backend:**
   - Build a **FastAPI backend** to manage the menu and orders. 
   - **API endpoints:** 
     - **POST** `/order` to submit a customer’s order.
     - **GET** `/menu` to retrieve the current menu for display.
     - **GET** `/orders` to retrieve orders for the restaurant side.

4. **Communication Between Frontend and Backend:**
   - Use HTTP requests (FastAPI) from Streamlit to communicate with the backend. For example, when a customer adds an item to the cart, send a **POST request** to the backend to store the order in the database.
   - Ensure real-time updates for the restaurant side using a method like **WebSockets** to push new orders to the restaurant frontend.

5. **Testing Locally:**
   - Run both the **Streamlit frontend** and **FastAPI backend** locally on your computer.
   - Test that the customer frontend can make orders, and the restaurant side can receive them and display them properly.
   - Test cross-computer communication in your local network by accessing the local development server from another device.

---

### **2. Segmentation: Backend and Frontend Repositories**
1. **Create Separate Repositories:**
   - Move your backend code to a **`restaurant-menu-backend`** repository.
   - Move your frontend code (Streamlit app) to a **`restaurant-menu-frontend`** repository.

2. **Set Up the Repositories:**
   - Each repository should have its own `requirements.txt`, Dockerfile, and appropriate environment configurations (e.g., CORS in FastAPI).
   - Push both repositories to GitHub or another version control system.

3. **Independent Testing:**
   - Ensure that each repository is **testable independently** by running their respective services (backend and frontend).
   - The frontend should make requests to the backend running locally to ensure both parts communicate properly.

---

### **3. Dockerization**
1. **Dockerize the Backend:**
   - Write a **Dockerfile** for the FastAPI backend.
   - Include all dependencies and set up the port for FastAPI to run (default is port 8000).

2. **Dockerize the Frontend:**
   - Write a **Dockerfile** for the Streamlit frontend.
   - Include all dependencies and set the default port for Streamlit (default is port 8501).

3. **Create Docker Compose File:**
   - Create a **`docker-compose.yml`** file to define both services (backend and frontend) and link them.
   - For example:
     ```yaml
     version: '3'
     services:
       frontend:
         build:
           context: ./restaurant-menu-frontend
         ports:
           - "8501:8501"
       backend:
         build:
           context: ./restaurant-menu-backend
         ports:
           - "8000:8000"
     ```

4. **Test Locally with Docker Compose:**
   - Run `docker-compose up` to start both services in Docker containers.
   - Ensure that the backend is accessible by the frontend, and test the communication between them.

---

### **4. Deployment on AWS**
1. **Prepare AWS Infrastructure:**
   - Set up an **EC2 instance** or use **ECS** for containerized deployments.
   - If using EC2, you’ll need to install Docker and Docker Compose on the instance.
   - Alternatively, use **ECS** with **Fargate** (for serverless container deployments).

2. **Push Docker Images to AWS ECR:**
   - Build Docker images for both the backend and frontend.
   - Push the images to **AWS Elastic Container Registry (ECR)**.

3. **Deploy Using AWS ECS or EC2:**
   - If using ECS with Fargate, define a service and task definition in ECS.
   - Set up a **load balancer** (ALB) to route traffic to the frontend (Streamlit) and backend (FastAPI).
   - Configure **security groups** and **IAM roles** for your services.
   - Use **ECS services** or EC2 instances to deploy the containers.

4. **Networking:**
   - Set up **VPC**, **subnets**, and **security groups** to ensure proper communication between the frontend and backend.
   - If using EC2, ensure you expose the required ports (8501 for Streamlit and 8000 for FastAPI).
   - Ensure **CORS** is configured correctly on the backend to accept requests from the frontend.

5. **Test the Deployed Application:**
   - Test that both frontend and backend are accessible from your browser.
   - Test the **order flow** and **real-time updates** to ensure everything is working as expected.