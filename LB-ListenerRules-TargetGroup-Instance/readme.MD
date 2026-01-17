![alt text](LB-ListenerRules-TargetGroup-Instance.drawio.svg)

## Load Balancer Request Flow
Frontend Port 80 → Nginx Port 80

In this architecture, the Load Balancer listens on port 80 and forwards requests to Nginx running on port 80 on EC2 instances.

1️⃣ User → Load Balancer (Port 80)

The user opens a browser and accesses:

http://example.com


The browser sends an HTTP request on port 80

The request reaches the public DNS/IP of the Load Balancer

📌 Frontend port = 80

2️⃣ Load Balancer Listener (Port 80)

The Load Balancer has a listener configured on port 80

The listener:

Accepts incoming HTTP requests

Evaluates listener rules

📌 A listener defines what action to take for requests coming to a specific port

3️⃣ Listener Rules → Target Group Selection

Listener rules may check:

Port (80)

Path (e.g., /, /login)

Host (e.g., www.example.com)

When a rule matches:

The request is forwarded to a Target Group

4️⃣ Listener → Target Group (Port 80)

The Target Group is configured with:

Protocol: HTTP

Port: 80

The Load Balancer forwards traffic to the target group on port 80

📌 Load Balancer port = 80
📌 Target Group port = 80

5️⃣ Target Group → Healthy EC2 Instances (Nginx on Port 80)

The Target Group:

Maintains a list of EC2 instances

Performs health checks on port 80

Only healthy instances receive traffic

On each EC2 instance:

Nginx is running on port 80

👉 Requests are forwarded only to healthy Nginx servers on port 80

6️⃣ Nginx → Response → User

Nginx processes the request

Sends the response back:

EC2 (Nginx:80) → Load Balancer → User


The user sees the final response in the browser

✅ One-Line Interview Explanation
User → Load Balancer (80)
→ Listener (80)
→ Target Group (80)
→ Healthy EC2 Instances running Nginx (80)
→ Response → User
