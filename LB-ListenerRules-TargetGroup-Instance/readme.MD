![alt text](LB-ListenerRules-TargetGroup-Instance.drawio.svg)

## Load Balancer Request Flow
Frontend Port 80 → Nginx Port 80

In this architecture, the Load Balancer listens on port 80 and forwards requests to Nginx running on port 80 on EC2 instances.

#### 1️⃣ User → Load Balancer (Port 80)

User opens browser and hits:

http://example.com


Browser sends an HTTP request on port 80

Request reaches the Application Load Balancer (public)

📌 Frontend Port = 80

#### 2️⃣ Load Balancer Listener (Port 80)

ALB has a Listener configured on port 80

Listener:

Receives the request

Evaluates listener rules

📌 Listener decides where to forward the request

#### 3️⃣ Listener Rules → Target Group (Port 80)

Listener rules may check:

Path ( / , /login, /api )

Host name ( optional )

When rule matches:

Request is forwarded to Target Group

📌 Target Group Port = 80

#### 4️⃣ Target Group → Healthy EC2 Instances (Nginx on 80)

Target Group:

Maintains list of EC2 instances

Performs health checks on port 80

Only healthy instances receive traffic

Nginx is running on port 80 in every instance

📌 No 8080 anywhere — only port 80

#### 5️⃣ Auto Scaling Group Behind Target Group

Target Group is attached to an Auto Scaling Group

ASG uses a Launch Template

Launch Template uses a prebuilt AMI with:

Nginx installed

Application configured

#### 6️⃣ High Traffic → Scale OUT (Automatic)

Traffic increases

ALB request count / CPU crosses threshold

Scaling policy triggers

ASG:

Launches new EC2 instances from AMI

Automatically registers them with Target Group

High traffic → New instances → Added to Target Group → Load balanced

#### 7️⃣ Low Traffic → Scale IN (Automatic)

Traffic decreases

Utilization goes below threshold

ASG:

Terminates extra EC2 instances

Deregisters them from Target Group

Low traffic → Instances terminated → Cost reduced

#### 8️⃣ Response Flow Back to User
EC2 (Nginx 80)
→ Target Group
→ Load Balancer
→ User Browser
