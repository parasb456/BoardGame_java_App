# BoardgameListingWebApp

## Description

**Board Game Database Full-Stack Web Application.**  
This web application allows users to explore lists of board games and their reviews.  
- Non-members can view the board game lists and reviews.  
- Authenticated users can add board games to the list and write reviews.  
- Managers have additional permissions to edit or delete reviews, along with the abilities granted to users.  

The application is deployed using Kubernetes on AWS infrastructure and supports continuous integration and monitoring features for a robust production setup.

---

## Technologies Used

### Backend & Core  
- **Java**  
- **Spring Boot** (for backend logic and APIs)  
- **Spring MVC** (for handling views, controllers, and database interaction)  
- **Spring Security** (for authentication and role-based access control)  
- **H2 Database** (In-memory database engine for development and testing)  
- **JDBC** (for database connectivity)  

### Frontend  
- **Thymeleaf** (for server-side rendering of HTML)  
- **Thymeleaf Fragments** (to reduce redundancy in templates)  
- **HTML5**, **CSS**, **JavaScript** (for UI and interactivity)  
- **Twitter Bootstrap** (for responsive design and styling)  

### Deployment & DevOps  
- **Amazon Web Services (AWS) EC2**  
  - EC2 instances for hosting Kubernetes (master and worker nodes)  
  - Additional EC2 instance for self-hosted GitHub Actions runner  
- **Kubernetes**  
  - Deployed the application using Kubernetes (Pods, Deployments, and Services)  
- **GitHub Actions** (for Continuous Integration and Continuous Deployment)  
- **Docker** (for containerizing the application)  

### Monitoring  
- **Prometheus** (metrics collection and alerting)  
- **Blackbox Exporter** (monitoring endpoints)  
- **Node Exporter** (monitoring server metrics)  
- **Grafana** (for visualizing metrics and dashboards)  

---

## Features

### General  
- Full-Stack Application with server-side rendered views  
- Responsive and user-friendly UI built with Bootstrap  

### Authentication and Authorization  
- **Authentication:** Login functionality with username and password  
- **Authorization:**  
  - **Non-members:** View board game lists and reviews  
  - **Users:** Add board games and write reviews  
  - **Managers:** Edit and delete reviews  

### Deployment & Infrastructure  
- Kubernetes cluster with master and worker nodes hosted on AWS EC2 instances  
- Self-hosted GitHub Actions runner for CI/CD pipeline  

### Monitoring & Metrics  
- Prometheus for monitoring application metrics  
- Blackbox Exporter for endpoint monitoring  
- Node Exporter for server-level metrics  
- Dashboards visualized using Grafana  

---

## How to Run

1. **Clone the Repository**  
   ```bash
   git clone https://github.com/your-repo-url/BoardgameListingWebApp.git
   cd BoardgameListingWebApp
   ```

2. **Build and Run Locally**  
   - Ensure you have Java and Maven installed.  
   - Run the application:  
     ```bash
     mvn spring-boot:run
     ```

3. **Kubernetes Deployment**  
   - Deploy the application to Kubernetes using the provided `deployment.yaml` and `service.yaml`.  
   - Access the application via the NodePort service on your EC2 public IP and port.

4. **Login Details**  
   Use the initial credentials for testing:  
   - **Username:** `bugs` | **Password:** `bunny` (User role)  
   - **Username:** `daffy` | **Password:** `duck` (Manager role)  
   - Or sign up as a new user.  

---

## Deployment Steps

1. **Clone the Repository**  
   ```bash
   git clone <repository-url>
   cd boardgame-app
   ```

2. **Pull the Docker Image**  
   The application image is available on Docker Hub:
   ```bash
   docker pull parasbhardwaj666/boardgame:9
   ```

3. **Deploy to Kubernetes**  
   Apply the Deployment and Service Manifest:

   Create a namespace and deploy resources:
   ```bash
   kubectl create namespace webapps
   kubectl apply -f deployment.yaml -n webapps
   kubectl apply -f service.yaml -n webapps
   ```

   Verify Deployment:
   ```bash
   kubectl get pods -n webapps
   kubectl get svc -n webapps
   ```

4. **Access the Application**  
   Identify the Node IP:
   ```bash
   kubectl get nodes -o wide
   ```

   Access the application using the Node IP and the exposed NodePort:
   ```
   http://<NodeIP>:30080
   ```
   Example: http://15.223.188.10:30080

---

## Kubernetes Details

- **Namespace:** `webapps`
- **Deployment:** `boardgame-deployment`
- **Pods:** 2 replicas for high availability.
- **Service:** NodePort service exposing port 30080 to map to container port 8080.

### Example Kubernetes YAML Files

#### Deployment File: `deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: boardgame-deployment
  namespace: webapps
spec:
  replicas: 2
  selector:
    matchLabels:
      app: boardgame
  template:
    metadata:
      labels:
        app: boardgame
    spec:
      containers:
      - name: boardgame
        image: parasbhardwaj666/boardgame:9
        ports:
        - containerPort: 8080
```

#### Service File: `service.yaml`
```yaml
apiVersion: v1
kind: Service
metadata:
  name: boardgame-ssvc
  namespace: webapps
spec:
  type: NodePort
  selector:
    app: boardgame
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
    nodePort: 30080
```

---

## Troubleshooting

### Application Not Accessible

Check Pod and Service Status:
```bash
kubectl get pods -n webapps
kubectl describe pod <pod-name> -n webapps
kubectl get svc -n webapps
```

Ensure the EC2 Security Group allows traffic on port 30080.

Verify the Node IP is reachable.

### Logs and Debugging

View application logs:
```bash
kubectl logs <pod-name> -n webapps
```

---

## Contributors

- **Paras Bhardwaj:** Application development and deployment.

---

## Screenshots


![Alt text](<Screenshot 2025-01-06 at 1.35.28 AM.png>)

![Alt text](<Screenshot 2025-01-06 at 1.35.39 AM.png>)

![Alt text](<Screenshot 2025-01-06 at 1.36.10 AM.png>)
---

