1. What are all the types of applications you have deployed?
● Answer: Web applications (e.g., Java Spring Boot, Node.js), microservices architectures,
databases (e.g., MySQL, MongoDB), message brokers (e.g., Apache Kafka), monitoring tools
(e.g., Prometheus, Grafana), CI/CD pipelines (e.g., Jenkins), and containerized applications
using Docker and Kubernetes.

3. How have you injected the secrets in ConfigMaps?
● Answer: Secrets should not be injected in ConfigMaps as ConfigMaps are not designed for
sensitive data. Instead, Kubernetes Secrets should be used. Secrets can be injected into
pods via environment variables or mounted as files.

5. How do you find which pod is taking more system resources across nodes
using kubectl?
● Answer: Use kubectl top pod --all-namespaces to list resource usage by pods.
Combine it with kubectl describe pod <pod-name> to get detailed resource usage

7. How do you know which worker node is consuming more resources across
the clusters using kubectl?
● Answer: Use kubectl top nodes to see resource consumption across nodes. This will
show CPU and memory usage on each node.

9. What are the steps for configuring Prometheus and Grafana for monitoring
Kubernetes clusters?
● Answer:
1. Deploy Prometheus using Helm or a custom YAML configuration.
2. Set up Kubernetes service discovery for Prometheus.
3. Deploy Grafana and configure it to use Prometheus as a data source.
4. Import Kubernetes monitoring dashboards in Grafana.
5. Set up alerting rules in Prometheus as needed.

6. 
7. If 20 pods are running, how do you visualize the metrics of these pods in
Grafana?
● Answer: Configure Grafana to use Prometheus as the data source. Use existing Kubernetes
dashboards or create custom dashboards to visualize metrics for all pods.
8. What is Apache Kafka?
● Answer: Apache Kafka is a distributed event streaming platform used for building real-time
data pipelines and streaming applications. It’s designed to handle large volumes of data in a
distributed, fault-tolerant manner.
9. How do you set up a Docker Hub private registry and integrate it with a CI/CD
pipeline? What is the procedure?
● Answer:
1. Create a private repository on Docker Hub.
2. Configure CI/CD pipeline to authenticate with Docker Hub using credentials.
3. Build Docker images in CI pipeline and push them to the private repository.
4. Pull images from the private registry during the CD process.
9. What is the difference between a hard link and a soft link?
● Answer:
○ Hard Link: A direct reference to the file’s data on the disk. Deleting the original file
does not affect the hard link.
○ Soft Link (Symbolic Link): A pointer to the original file’s path. If the original file is
deleted, the soft link becomes broken.
10. What is the use of the break command in shell scripting? In what scenarios
have you used it?
- **Answer:** The `break` command is used to exit a loop prematurely. It is commonly used when
a specific condition is met, and you no longer need to continue the loop.
11. How do you count the number of "devops" words in 15 HTML files?
To count the number of times the word "devops" appears across 15 HTML files, you can use the
following shell command:
grep -o -i "devops" *.html | wc -l
Explanation:
● grep -o -i "devops" *.html: Searches for the word "devops" (case-insensitive due to
-i) in all HTML files and outputs each occurrence on a new line (-o).
● wc -l: Counts the number of lines output by grep, which corresponds to the number of
occurrences of "devops".
12. What is the terraform taint command?
The terraform taint command is used to manually mark a specific resource for recreation
during the next terraform apply. When a resource is tainted, Terraform will destroy it and create
a new instance of it on the next apply. This is useful when you know a resource needs to be replaced
but Terraform hasn't automatically determined that it should be.
Syntax: terraform taint <resource_address>
Example: terraform taint aws_instance.example
13. What are the possible ways to secure a state file in Terraform?
1. Encrypt the State File: Store the state file in an encrypted format using tools like AWS S3
bucket encryption, Azure Blob encryption, or Google Cloud Storage encryption.
2. Use Remote Backends with Security Controls: Use a remote backend like AWS S3 with IAM
roles and policies, HashiCorp Consul with ACLs, or Terraform Cloud, which manages access
control and encryption.
3. Enable Versioning: Use versioning on the state file storage to recover from any unintended
changes or deletions.
4. Access Control: Restrict access to the state file using IAM policies, RBAC (Role-Based
Access Control), or similar mechanisms.
5. Use Backend Locking: Use state locking mechanisms provided by backends like S3 with
DynamoDB or Consul to prevent concurrent operations from corrupting the state file.
14. If you provision 100 servers and someone deletes 50 VMs manually, what
happens if you apply the terraform apply command?
When you run terraform apply, Terraform will compare the state file with the actual
infrastructure. It will notice that 50 VMs are missing and will attempt to recreate those missing VMs
to match the state defined in your configuration. The end result will be 100 VMs again.
15. What is the syntax for for_each in Terraform?
The for_each meta-argument in Terraform allows you to create multiple instances of a resource or
module based on the items in a map or set. The syntax is as follows:
resource "aws_instance" "example" {
 for_each = var.instances
 ami = each.value["ami"]
 instance_type = each.value["instance_type"]
 tags = {
 Name = each.key
 }
}
In this example, var.instances is a map, and each.key refers to the current key in the map,
while each.value refers to the associated value.
16. What are the advantages and disadvantages of multi-stage builds in
Docker?
Advantages:
● Smaller Image Size: By separating the build environment from the runtime environment, only
the necessary components are included in the final image, leading to smaller image sizes.
● Improved Security: Reduces the attack surface by excluding build tools and other
unnecessary components from the final image.
● Better Caching: Allows for more efficient use of Docker’s caching mechanism, potentially
speeding up the build process.
● Separation of Concerns: Different stages can focus on specific tasks, making the Dockerfile
more organized and easier to maintain.
Disadvantages:
● Complexity: Multi-stage Dockerfiles can be more complex and harder to understand,
especially for those new to Docker.
● Longer Build Times: In some cases, the use of multiple stages may lead to longer build
times due to additional steps and transitions between stages.
● Troubleshooting: Debugging and troubleshooting can be more challenging because
intermediate stages are not preserved by default.
17. How do you deploy containers on different hosts, not the same host, within
a Docker cluster?
To deploy containers on different hosts within a Docker cluster, you can use Docker Swarm or
Kubernetes:
● Docker Swarm: Use Docker Swarm’s built-in orchestration capabilities. When deploying a
service, Swarm will automatically distribute the containers across different nodes in the
cluster. You can influence this behavior using placement constraints.
Example:
docker service create --name myservice --replicas 2 --constraint 'node.role == worker' myimage
Kubernetes: Use Kubernetes, which will schedule pods on different nodes based on the available
resources and any specified node selectors or affinities.
18. If you have a Docker Compose setup, how do you deploy the web container
on one host and the DB container on another host?
To deploy the web container on one host and the DB container on another using Docker Compose,
you can:
1. Use Docker Swarm: Convert the Compose file into a stack file, and deploy it with docker
stack deploy. Docker Swarm will distribute services across nodes.
2. Manual Placement: Use placement constraints in your docker-compose.yml file to
specify which containers should run on which nodes.
version: '3.7'
services:
 web:
 image: my-web-image
 deploy:
 placement:
 constraints: [node.hostname == web-node]
 db:
 image: my-db-image
 deploy:
 placement:
 constraints: [node.hostname == db-node]
19. What is the difference between bridge networking and host networking in
Docker?
● Bridge Networking: The default Docker network driver. Containers connected to the same
bridge network can communicate with each other. Each container gets its own IP address
and is isolated from the host network. You can expose ports to the host using the -p option.
● Host Networking: The container shares the host's network stack, meaning it doesn’t get its
own IP address, and the container's network is the same as the host's network. This can lead
to performance improvements but reduces isolation between the host and the container.
20. How do you resolve merge conflicts?
To resolve merge conflicts, follow these steps:
1. Identify the Conflict: Git will mark the conflicting areas in the files with conflict markers
(<<<<<<<, =======, and >>>>>>>).
2. Manually Resolve: Open the conflicting file and decide how to combine the changes.
Remove the conflict markers and modify the content to resolve the conflict.
3. Stage the Resolved Files: Once resolved, stage the files using git add.
4. Commit the Changes: Commit the resolved conflicts with git commit. If you were in the
middle of a merge, this will complete the merge process.
5. Test: Run tests to ensure that the merge didn’t introduce any issues.
Example:
git add <resolved-files>
git commit -m "Resolved merge conflicts in X, Y, Z files"
21. What command do you use to change the existing commit message?
To change the most recent commit message, you can use the following Git command:
git commit --amend -m "New commit message"
If you have already pushed the commit to a remote repository, you will need to force push the
changes:
git push --force
22. What is session affinity?
Session affinity, also known as sticky sessions, is a concept in load balancing where requests from a
particular user are consistently directed to the same server (or pod) in a multi-server environment.
This ensures that the user's session data, which might be stored locally on the server, remains
accessible throughout the session.
23. What is pod affinity and its use case?
Pod affinity is a feature in Kubernetes that allows you to specify rules for scheduling pods to run on
nodes that have other specified pods running on them. This can be useful when you want certain
pods to be located together due to factors like data locality, network latency, or shared resources.
Use Case: An application where the frontend and backend services communicate frequently might
use pod affinity to ensure that both are scheduled on the same node to reduce network latency.
24. What is the difference between pod affinity and pod anti-affinity?
● Pod Affinity: Ensures that pods are scheduled on the same node or in proximity to each
other.
● Pod Anti-Affinity: Ensures that pods are not scheduled on the same node or are placed far
apart from each other.
Example Use Case:
● Pod Affinity: Running frontend and backend on the same node for low-latency
communication.
● Pod Anti-Affinity: Ensuring replicas of the same application are spread across different
nodes to increase availability and fault tolerance.
25. What are readiness and liveness probes?
● Readiness Probe: Used to determine when a pod is ready to start accepting traffic. If the
readiness probe fails, the pod will be removed from the service endpoints, ensuring it does
not receive traffic until it's ready.
● Liveness Probe: Used to determine if a pod is still running. If the liveness probe fails,
Kubernetes will restart the pod, assuming it's in a failed state.
26. Write a simple Groovy pipeline for a Java Spring Boot application that waits
for user input for approval to move to the next stage, with stages for checkout,
build, push, and deploy.
pipeline {
 agent any
 stages {
 stage('Checkout') {
 steps {
 git 'https://github.com/your-repo/java-spring-boot-app.git'
 }
 }
 stage('Build') {
 steps {
 sh './mvnw clean package'
 }
 }
 stage('Push') {
 steps {
 sh 'docker build -t your-docker-repo/java-spring-boot-app .'
 sh 'docker push your-docker-repo/java-spring-boot-app'
 }
 }
 stage('Approval') {
 steps {
 input 'Do you want to deploy to production?'
 }
 }
 stage('Deploy') {
 steps {
 sh 'kubectl apply -f deployment.yaml'
 }
 }
 }
}
27. How do you export test reports in Jenkins?
To export test reports in Jenkins:
1. Ensure that your tests generate reports in a standard format like JUnit XML or HTML.
2. Use the Publish JUnit test result report post-build action to archive test reports.
3. You can also use the Archive the artifacts post-build action to store other types of reports.
4. The test reports can then be accessed and downloaded from the Jenkins build page.
28. If 5 pods are running, how do you scale the number of pods to 10 using the
command line in Kubernetes?
To scale the number of pods from 5 to 10, use the following command:
kubectl scale --replicas=10 deployment/<your-deployment-name>
29. Can you explain the usage of the terraform import command?
The terraform import command is used to import existing infrastructure resources into
Terraform's state file, allowing Terraform to manage them. This is particularly useful when you want
to bring existing infrastructure under Terraform management without having to recreate resources.
terraform import <resource_type>.<resource_name> <resource_id>
Example:
terraform import aws_instance.my_instance i-1234567890abcdef0
30. In AWS, where do you store the state file, and how do you manage it?
In AWS, Terraform state files are typically stored in an S3 bucket, and the state can be managed
using a combination of S3 and DynamoDB for locking.
Example Configuration:
terraform {
 backend "s3" {
 bucket = "my-terraform-state-bucket"
 key = "path/to/my/terraform.tfstate"
 region = "us-west-2"
 dynamodb_table = "terraform-lock-table"
 encrypt = true
 }
}
Management:
● S3: Stores the state file securely, supports versioning, and can be encrypted.
● DynamoDB: Provides locking to prevent multiple simultaneous operations on the same state
file, avoiding conflicts.
This setup ensures that your Terraform state is stored securely and is protected from simultaneous
access issues.
31. What is the biggest issue you have faced with Terraform, and how did you resolve it?
● One significant issue I faced was managing Terraform state when multiple teams were
working on the same infrastructure. This was resolved by organizing the infrastructure into
separate modules and using remote state management with proper locking mechanisms.
32. What are the types of storage accounts in AWS S3?
● In AWS S3, the different storage classes include:
○ S3 Standard
○ S3 Intelligent-Tiering
○ S3 Standard-IA (Infrequent Access)
○ S3 One Zone-IA
○ S3 Glacier
○ S3 Glacier Deep Archive
33. Are you familiar with lifecycle management in S3 buckets? How do you set up lifecycle
policies?
● Yes, lifecycle management in S3 allows you to define rules to transition objects between
different storage classes or delete them after a certain period. Lifecycle policies can be set
up using the S3 Management Console, AWS CLI, or Terraform by specifying the transitions
and expiration actions in a JSON configuration file.
34. What are the differences between load balancers, and why do we need them?
● Load balancers distribute incoming network traffic across multiple servers. The main types
are:
○ Application Load Balancer (ALB): Operates at the application layer (Layer 7), and is
used for HTTP/HTTPS traffic with advanced routing capabilities.
○ Network Load Balancer (NLB): Operates at the transport layer (Layer 4) and is used
for ultra-low latency TCP/UDP traffic.
○ Classic Load Balancer (CLB): Supports both Layer 4 and Layer 7, but is now mostly
deprecated in favor of ALB and NLB.
● Load balancers improve fault tolerance, scalability, and ensure high availability.
35. Have you worked with Auto Scaling Groups (ASG)?
● Yes, I have worked with ASGs to automatically scale the number of instances in response to
demand. ASGs are configured with policies that adjust the desired capacity based on
metrics such as CPU utilization, helping to maintain application performance and optimize
costs.
36. Can you write a simple Dockerfile?
Yes, here is a simple example of a Dockerfile for a Node.js application:
Dockerfile
FROM node:14
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
37. If you want to expose your application to the public internet or access your application within a
cluster, how would you do that in Kubernetes?
● To expose your application to the public internet, you can use a Kubernetes Service of type
LoadBalancer or NodePort. For internal access within the cluster, you can use a
ClusterIP service. Additionally, you might use an Ingress controller for more advanced
routing.
38. Why do we need a ConfigMap in Kubernetes?
● A ConfigMap is used to store non-confidential configuration data in key-value pairs. It allows
you to decouple configuration artifacts from image content, enabling you to modify
application settings without rebuilding your container images.
39. Which AWS services do you consider when setting up a CI/CD pipeline for a microservices
application?
● For a CI/CD pipeline, I would consider using:
○ AWS CodeCommit for source control.
○ AWS CodeBuild for building and testing.
○ AWS CodeDeploy or Amazon EKS for deployment.
○ AWS CodePipeline to orchestrate the CI/CD process.
○ Amazon S3 for artifact storage.
○ AWS Lambda for any custom automation tasks.
40. On a day with unusually high traffic for an e-commerce application, how would you, as a cloud
engineer, manage the current setup to handle the load smoothly?
● I would ensure that Auto Scaling Groups are properly configured to handle the increased
demand by automatically adding more instances. I'd also verify that the load balancers are
evenly distributing traffic and consider enabling caching (e.g., using Amazon CloudFront) to
reduce the load on backend servers. Monitoring tools like CloudWatch would be used to
track performance metrics and adjust resources in real-time.
● Strategies:
○ Auto Scaling: Scale up instances automatically to handle the increased load.
○ Caching: Use a caching layer to reduce the load on your application servers.
○ Load Balancing: Distribute traffic evenly across available instances.
○ Database Optimization: Ensure your database is properly configured and optimized for
performance.
○ Monitoring: Closely monitor system metrics to identify bottlenecks and adjust resources
accordingly.
41. If traffic is currently handled on a single instance, how would you upgrade for high availability
in AWS?
● To upgrade for high availability, I would:
○ Deploy multiple instances across different Availability Zones (AZs) using an Auto
Scaling Group.
○ Set up a Load Balancer (ALB or NLB) to distribute traffic across these instances.
○ Configure health checks to ensure traffic is only routed to healthy instances.
○ Use Multi-AZ deployments for databases like RDS to ensure data availability.
42. When auto-scaling instances, how do you manage the backend RDS database?
● To manage the backend RDS database during auto-scaling:
○ Enable Multi-AZ for high availability and automatic failover.
○ Use RDS Read Replicas to handle read-heavy traffic, reducing the load on the primary
database.
○ Scale RDS vertically (instance size) or horizontally (read replicas) based on the
database workload.
○ Monitor performance using Amazon CloudWatch and adjust as necessary.
43. Have you ever set up cross-account access for S3? For example, if the QA team needs access
to the production database.
● Yes, I've set up cross-account access by:
○ Creating an IAM role in the production account with the necessary S3 permissions.
○ Establishing a trust relationship to allow the QA account to assume that role.
○ Using S3 bucket policies to grant access from the QA account.
○ QA team members can then assume the role using AWS STS (Security Token
Service) to access the production S3 bucket.
44. How can an S3 account in Account A access an S3 account in Account B?
● Account A can access Account B’s S3 bucket by:
○ Setting up a bucket policy in Account B that grants Account A the necessary
permissions.
○ Creating an IAM role in Account B with permissions for S3 and allowing Account A to
assume that role via a trust policy.
○ Using AWS STS to assume the role from Account A and access the S3 bucket in
Account B.
45. Can you differentiate between IAM policies and IAM roles?
● IAM Policies: These are sets of permissions attached to users, groups, or roles, defining
what actions are allowed or denied.
● IAM Roles: These are identities with specific permissions that can be assumed by entities
like users, applications, or AWS services. Roles are often used for temporary access or
cross-account access.
46. Can you explain the STS assume role policy?
● The STS (Security Token Service) AssumeRole policy allows a user or service to assume a
role in a different account or within the same account. This provides temporary security
credentials with the permissions associated with the assumed role, enabling cross-account
access or delegation of permissions.
47. Have you experienced any challenging issues or incidents in your project? How did you and
your team identify and resolve them?
● Yes, one challenge was a sudden traffic spike causing performance degradation. We
identified the issue using CloudWatch metrics and logs, pinpointing the bottleneck in the
database. The resolution involved scaling the database vertically and adding read replicas to
distribute the load, along with optimizing slow-running queries.
48. What is the difference between CMD and ENTRYPOINT in Docker?
● CMD: Provides default arguments for the entrypoint or the command to run if no other
command is provided.
● ENTRYPOINT: Defines the executable that will always run, with CMD as its default
arguments. ENTRYPOINT is useful when you want your container to behave like a specific
executable.
● Example: ENTRYPOINT ["python", "app.py"] ensures app.py is always executed,
while CMD allows passing different arguments.
49. Have you ever managed an application single-handedly?
● Yes, I have managed applications single-handedly, handling tasks such as deployment,
monitoring, troubleshooting, and scaling. This involved setting up the infrastructure, CI/CD
pipelines, and ensuring high availability and security.
50. What are the benefits of Infrastructure as Code (IaC)?
● Consistency: Ensures consistent environments by automating the provisioning process.
● Version Control: Infrastructure can be versioned and tracked, enabling rollbacks and audits.
● Automation: Reduces manual intervention, minimizing errors and speeding up deployments.
● Scalability: Allows easy scaling and management of resources through scripts.
● Collaboration: Teams can collaborate more effectively using code reviews and version
control systems.
51. What are the different ways to create infrastructure as code?
● Terraform: A popular open-source tool that allows you to define infrastructure as code using
a declarative configuration language. It works with various cloud providers.
● AWS CloudFormation: A service provided by AWS that enables you to define AWS resources
using JSON or YAML templates.
● Azure Resource Manager (ARM) Templates: Azure's solution for infrastructure as code,
allowing you to define resources in JSON.
● Google Cloud Deployment Manager: Google's infrastructure as code tool that uses YAML to
define resources.
● Ansible: Although primarily a configuration management tool, it can also be used to
provision infrastructure using playbooks written in YAML.
● Pulumi: A modern infrastructure as code tool that supports multiple programming languages
like Python, JavaScript, and Go.
52. What is the difference between public and private networking?
● Public Networking: Refers to networks that are accessible from the internet. Public IP
addresses are used, and resources are exposed to external access.
● Private Networking: Refers to networks that are isolated from the public internet. Resources
within a private network communicate with each other securely using private IP addresses,
and access from outside is typically restricted.
53. What is a Docker registry and why do we need it?
● Docker Registry: A storage and distribution system for Docker images. It allows you to store,
share, and manage Docker container images. Docker Hub is a popular public registry, but you
can also set up private registries.
● Why We Need It: Docker registries allow teams to version, share, and deploy Docker images
easily. They support CI/CD pipelines by enabling automated builds and deployments.
54. What is a secrets manager?
● Secrets Manager: A tool or service that securely stores and manages sensitive information
such as API keys, passwords, certificates, and tokens. Examples include AWS Secrets
Manager, HashiCorp Vault, and Azure Key Vault.
● Purpose: To securely store and access secrets without hardcoding them into application
code or configuration files.
55. What is the secure way to manage sensitive information?
● Use a Secrets Manager: Store secrets in a dedicated service like AWS Secrets Manager,
Azure Key Vault, or HashiCorp Vault.
● Environment Variables: Use environment variables to inject secrets at runtime rather than
storing them in code.
● Encrypted Storage: Store sensitive data in encrypted databases or files, ensuring that
encryption keys are managed securely.
● Access Control: Implement strict access controls and auditing to ensure that only authorized
personnel and applications can access sensitive information.
56. Have you worked with Kubernetes (K8s)?
If you have experience with Kubernetes, you might discuss:
● Deploying and managing containerized applications using Kubernetes.
● Configuring Kubernetes clusters and using tools like kubectl.
● Managing services, ingress, and networking in Kubernetes.
● Using Helm charts for packaging and deploying applications.
57. What is the difference between Docker and Kubernetes?
● Docker: A platform for developing, shipping, and running applications inside containers. It
simplifies the process of managing application dependencies and environment consistency.
● Kubernetes: An open-source orchestration system for automating the deployment, scaling,
and management of containerized applications. Kubernetes manages multiple containers
across a cluster, providing features like load balancing, scaling, and self-healing.
58. Can you explain an end-to-end deployment for an application?
An end-to-end deployment process might involve the following steps:
1. Code Commit: Developers push code changes to a version control system like Git.
2. CI/CD Pipeline: A continuous integration pipeline builds the code, runs tests, and creates a
Docker image.
3. Image Storage: The Docker image is pushed to a Docker registry.
4. Deployment: The image is pulled from the registry by Kubernetes, Docker Swarm, or another
orchestration tool, and deployed to a staging or production environment.
5. Monitoring & Logging: The application is monitored for performance and errors, with logs
collected and analyzed.
6. Scaling & Updates: The application is scaled based on demand, and updates are rolled out
using a strategy like blue-green or canary deployment.
59. If you want to use Kubernetes instead of EC2 instances, how would
you do it? Have you used Helm charts or other CD tools? How would you
handle a project with multiple microservices on Kubernetes?
● Using Kubernetes Instead of EC2: You would deploy your applications on a Kubernetes
cluster rather than directly on EC2 instances. This involves setting up an EKS (Elastic
Kubernetes Service) cluster in AWS or using another managed Kubernetes service.
● Helm Charts: Helm is a package manager for Kubernetes that helps you manage Kubernetes
applications. You can use Helm charts to deploy and manage multiple microservices in a
consistent and repeatable manner.
● Handling Multiple Microservices: Use Kubernetes namespaces to isolate microservices, and
manage their deployment using Helm charts or a CI/CD tool like Jenkins, ArgoCD, or GitLab
CI/CD. Implement service discovery, networking, and security policies to ensure seamless
communication between microservices.
60. How do you connect a bastion host to a private network? Can you
explain VPC and VPC peering?
● Connecting a Bastion Host: A bastion host is typically set up in a public subnet of a Virtual
Private Cloud (VPC) with access to the private network. Users connect to the bastion host
via SSH, and from there, they can access resources in the private subnet.
● VPC (Virtual Private Cloud): A logically isolated section of a cloud provider's network where
you can launch and manage resources. It allows you to define IP ranges, subnets, route
tables, and network gateways.
● VPC Peering: A network connection between two VPCs that allows traffic to be routed
between them using private IP addresses. This is useful for connecting resources across
different VPCs without going over the public internet.
61. Have you configured a system where code is automatically merged and published upon a
developer completing a ticket in Jira? What exactly have you managed?
Yes, I have set up a CI/CD pipeline where code is automatically merged and published once a
developer completes a ticket in Jira. This process typically involves:
● Integration with Jira: Configuring Jira to trigger CI/CD pipelines when a ticket is marked as
"Done" or moved to a specific workflow stage.
● Code Merging: Using tools like GitLab CI, GitHub Actions, or Jenkins to automatically merge
feature branches into the main branch after passing tests.
● Automated Testing: Running unit, integration, and end-to-end tests to ensure the quality of
the code before merging.
● Deployment: Using deployment tools like Kubernetes, Helm, or Docker Swarm to
automatically deploy the merged code to staging or production environments.
62. How do you set up Nginx on a server?
Setting up Nginx on a server involves:
1. Installation:
○ For Ubuntu/Debian: sudo apt-get update && sudo apt-get install
nginx
○ For CentOS/RHEL: sudo yum install nginx
2. Configuration:
○ Edit the configuration file located at /etc/nginx/nginx.conf or individual site
configurations in /etc/nginx/sites-available/.
○ Define server blocks (virtual hosts) to specify different sites and their root
directories.
○ Configure reverse proxy, load balancing, SSL/TLS certificates, and caching as
needed.
3. Start and Enable Nginx:
○ Start the service: sudo systemctl start nginx
○ Enable it to start on boot: sudo systemctl enable nginx
4. Testing:
○ Test the configuration: sudo nginx -t
○ Ensure Nginx is running and properly serving content.
63. What is a load balancer and its benefits? What is Cloud NAT?
● Load Balancer: A load balancer distributes incoming network traffic across multiple servers
or services to ensure reliability, scalability, and high availability. Benefits include:
○ Increased Fault Tolerance: Distributes traffic to prevent overload on a single server.
○ Scalability: Easily manage increased traffic by adding more servers.
○ Improved Performance: Balances load based on performance metrics, reducing
latency.
● Cloud NAT: Network Address Translation (NAT) service in cloud environments like Google
Cloud. It allows instances in private subnets to connect to the internet without exposing
them to inbound internet traffic, maintaining security while enabling outbound connectivity.
64. What is the difference between a load balancer and a Cloud NAT gateway?
● Load Balancer:
○ Distributes incoming traffic across multiple servers or services.
○ Primarily used for load distribution, redundancy, and high availability.
○ Works at various layers (L4 for TCP, L7 for HTTP/HTTPS).
● Cloud NAT Gateway:
○ Provides outbound internet access for instances in a private network without
exposing them to inbound traffic.
○ Used for secure, private instances that need internet access without being directly
accessible from the internet.
65. How do you see yourself fitting into this particular role?
I see myself fitting into this role by leveraging my technical expertise in infrastructure management,
automation, and cloud technologies. My experience in setting up CI/CD pipelines, managing
deployments, and optimizing resource allocation aligns with the responsibilities of this role. I also
bring problem-solving skills and a proactive approach to ensuring system reliability, which will
contribute to the success of the team and organization.
66. Can you share an instance where you provided a solution for cost optimization while managing
resource allocation?
In a previous project, I noticed that our cloud infrastructure was over-provisioned, leading to
unnecessary costs. I implemented auto-scaling based on actual usage metrics and utilized spot
instances for non-critical workloads. Additionally, I restructured the storage solution by moving
infrequently accessed data to lower-cost storage classes. These changes resulted in a significant
reduction in our monthly cloud expenses without compromising performance.
67. Describe a situation where the entire production instance crashed, and you had to fix it quickly.
Have you experienced such a scenario?
Yes, I have experienced such a scenario. In one instance, our production server crashed due to a
memory leak in the application. I quickly identified the issue using monitoring tools like Prometheus
and logs from ELK Stack. To resolve it, I restarted the affected services and temporarily scaled up
the infrastructure to handle the load. I then worked with the development team to identify and fix the
memory leak, ensuring it didn’t happen again.
68. What is blue-green deployment and why is it needed?
● Blue-Green Deployment: A deployment strategy where two identical environments (Blue and
Green) are maintained. The Blue environment is the active production environment, while the
Green is the idle one. During deployment, the new version is deployed to the Green
environment. After testing, traffic is switched to Green, making it the new production
environment.
● Why Needed:
○ Minimal Downtime: Reduces downtime as the switch between environments is
instantaneous.
○ Easy Rollback: If issues arise, switching back to the Blue environment is
straightforward.
○ Improved Reliability: Reduces the risk of deployment failures affecting users.
69. What other deployment strategies do you know?
● Canary Deployment: Gradually rolling out the new version to a small subset of users before a
full deployment.
● Rolling Deployment: Incrementally updating instances or servers with the new version,
ensuring at least some instances are always running the old version.
● A/B Testing: Similar to blue-green, but used for comparing different versions/features with
live user traffic to determine which performs better.
● Feature Toggles: Allows features to be turned on/off dynamically, enabling deployment of
incomplete features without impacting the user.
70. What advanced AWS resource types have you worked with and utilized?
I have worked with several advanced AWS resource types, including:
● AWS Lambda: Serverless computing for running code in response to events without
managing servers.
● AWS Fargate: Serverless compute engine for containers, allowing the deployment of
containerized applications without managing the underlying infrastructure.
● Amazon RDS: Managed database service for relational databases, including features like
automated backups, scaling, and multi-AZ deployments.
● AWS CloudFormation: Infrastructure as Code (IaC) tool for automating resource provisioning
and management.
● Amazon VPC Peering and Transit Gateway: For creating complex network architectures
across multiple VPCs and accounts.
● AWS Step Functions: Orchestration service for combining AWS Lambda functions and other
services into serverless workflows.
71. How are hosted modules (like AI/ML) deployed, customized, and scaled as per different
frontend/backend requirements in AWS with the help of a DevOps engineer?
Deploying, Customizing, and Scaling Hosted Modules in AWS
● Deployment: DevOps engineers use tools like AWS CodePipeline to automate the deployment of
hosted AI/ML modules. This involves:
● Containerization: Packaging the modules into Docker containers for portability.
● Infrastructure Provisioning: Using IaC (e.g., CloudFormation) to set up the necessary
AWS resources (EC2 instances, S3 buckets, etc.).
● Deployment Automation: Using tools like AWS CodeDeploy to deploy the containers to
the provisioned infrastructure.
● Customization: Customization often involves:
● Configuration Management: Using tools like Ansible or Puppet to configure the modules
based on specific requirements.
● Environment Variables: Using environment variables to inject different configurations for
different environments (development, testing, production).
● Scaling: DevOps engineers leverage AWS services like:
● Auto Scaling Groups: Automatically adjust the number of instances based on load.
● Elastic Load Balancing: Distribute traffic across multiple instances for high availability.
● Serverless Computing: Using services like AWS Lambda to scale AI/ML workloads
dynamically.
72. Can you describe a technology you had not heard of before but managed to learn and use on your
own?
Learning a New Technology
● Example: I recently learned about Apache Kafka, a distributed streaming platform. I was initially
unfamiliar with its concepts but was intrigued by its potential for real-time data processing.
● Learning Process: I started by reading documentation, watching tutorials, and experimenting with
Kafka on my own. I also joined online communities and forums to connect with other users and
learn from their experiences.
● Application: I successfully implemented Kafka in a project to handle high-volume event streams,
improving data processing efficiency and real-time insights.
73. What challenges have you faced as a DevOps engineer?
DevOps Engineer Challenges
● Complexity: Managing complex cloud environments with multiple services and dependencies.
● Automation: Finding the right balance between automation and manual intervention.
● Security: Ensuring the security of cloud infrastructure and applications.
● Collaboration: Working effectively with developers, operations teams, and other stakeholders.
● Continuous Learning: Keeping up with the rapid pace of change in the cloud computing
landscape.
74.Can you share real-life incidents where you solved an error after working for two or three days?
Real-Life Incident Resolution
● Incident: A recent incident involved a production application experiencing slow performance due
to a database query bottleneck.
● Resolution: I spent two days analyzing logs, profiling the database, and identifying the inefficient
query. I then worked with the development team to optimize the query, resulting in a significant
performance improvement.
75.Have you managed large-scale databases and real-time backups or replication?
Large-Scale Databases and Backups
● Experience: Yes, I have managed large-scale databases like MySQL and PostgreSQL, including
real-time backups and replication.
● Tools: I've used tools like Percona XtraBackup for MySQL backups and pg_dump for PostgreSQL
backups. I've also implemented replication using tools like MySQL replication and PostgreSQL
streaming replication.
76. During data loss, what strategy do you use to ensure no data loss, especially in critical applications
like banking?
Data Loss Prevention Strategy
● Strategy: For critical applications like banking, a multi-layered approach is essential:
● Redundancy: Use multiple data centers or cloud regions for replication and failover.
● Backups: Implement frequent and automated backups to multiple locations.
● Version Control: Track changes to data and maintain historical versions.
● Monitoring: Monitor database health and performance to detect potential issues early.
● Disaster Recovery Plan: Develop a comprehensive disaster recovery plan to restore data
and services in case of an outage.
77.Have you faced any cyberattacks on systems you built and implemented? What precautions do you
take?
Cyberattacks and Precautions
● Experience: I've encountered security incidents like brute-force attacks and attempts to exploit
vulnerabilities.
● Precautions:
● Security Best Practices: Implement strong passwords, multi-factor authentication, and
least privilege access.
● Vulnerability Scanning: Regularly scan systems for vulnerabilities and patch them
promptly.
● Security Monitoring: Use security information and event management (SIEM) tools to
monitor for suspicious activity.
● Incident Response Plan: Develop a plan to respond to security incidents effectively.
78. What are the networking setup rules you follow?
Networking Setup Rules
● Rules:
● Security Groups: Use security groups to control inbound and outbound traffic to
instances.
● Network Segmentation: Divide the network into smaller segments to isolate resources
and limit the impact of security breaches.
● Firewall Rules: Implement firewall rules to block unauthorized access.
● VPN and Tunneling: Use VPNs and tunnels to secure communication between networks.
● Network Monitoring: Monitor network traffic and performance to identify potential
issues.
79. What are your daily responsibilities as a DevOps engineer?
Daily Responsibilities
● Monitoring: Monitor system health, performance, and security.
● Automation: Automate tasks like deployments, infrastructure provisioning, and backups.
● Troubleshooting: Resolve issues and incidents.
● Collaboration: Work with developers, operations teams, and other stakeholders.
● Continuous Improvement: Identify areas for improvement and implement changes to enhance
efficiency and reliability.
80. Which DevOps tools are you proficient with?
DevOps Tools Proficiency
● Infrastructure as Code: Terraform, CloudFormation, Ansible, Puppet.
● Containerization: Docker, Kubernetes.
● CI/CD: Jenkins, GitLab CI/CD, AWS CodePipeline.
● Monitoring and Logging: Prometheus, Grafana, ELK Stack.
● Configuration Management: Ansible, Puppet, Chef.
● Version Control: Git.
81. Can you describe the CI/CD workflow in your project?
CI/CD Workflow Description
● Example: In a recent project, our CI/CD workflow involved:
1. Code Commit: Developers commit code changes to a Git repository.
2. Build and Test: A CI server (e.g., Jenkins, GitLab CI) automatically builds the application,
runs unit tests, and performs code quality checks.
3. Artifact Storage: Successful builds are stored as artifacts in a repository (e.g., S3).
4. Deployment: The CD server (e.g., AWS CodeDeploy) deploys the artifact to the target
environment (development, testing, production).
5. Monitoring: Continuous monitoring tools (e.g., Prometheus, Grafana) track application
health and performance.
82. How do you handle the continuous delivery (CD) aspect in your projects?
Continuous Delivery (CD) Handling
● Methods:
● Automated Deployments: Use tools like AWS CodeDeploy to automate deployments to
different environments.
● Blue-Green Deployments: Deploy new versions of the application alongside the existing
version, allowing for seamless switchover.
● Canary Deployments: Gradually roll out new versions to a small subset of users,
monitoring for issues before full deployment.
● Feature Flags: Use feature flags to enable or disable features in the application without
code changes, allowing for controlled releases.
83. What methods do you use to check for code vulnerabilities?
Code Vulnerability Checks
● Methods:
● Static Code Analysis: Use tools like SonarQube or Snyk to analyze code for vulnerabilities
and security issues.
● Dynamic Code Analysis: Use tools like Burp Suite or ZAP to test the application in
runtime for vulnerabilities.
● Security Scanning: Use tools like AWS Inspector or Qualys to scan infrastructure and
applications for vulnerabilities.
84. What AWS services are you proficient in?
AWS Service Proficiency
● Services:
● Compute: EC2, Lambda, ECS, EKS.
● Storage: S3, EBS, EFS.
● Networking: VPC, Route 53, Load Balancers.
● Database: RDS, DynamoDB, Redshift.
● CI/CD: CodePipeline, CodeBuild, CodeDeploy.
● Security: IAM, Security Groups, KMS.
● Monitoring: CloudWatch, CloudTrail.
85. How would you access data in an S3 bucket from Account A when your application is running on an
EC2 instance in Account B?
Accessing S3 from Account B
● Method: Use IAM roles and cross-account permissions:
1. Create Role: In Account A, create an IAM role with permissions to access the S3 bucket.
2. Assume Role: In Account B, configure the EC2 instance to assume the role created in
Account A.
3. Access S3: The EC2 instance can now access the S3 bucket using the assumed role's
credentials.
86. How do you provide access to an S3 bucket, and what permissions need to be set on the bucket side?
S3 Bucket Access and Permissions
● Access: You can provide access to an S3 bucket using:
● IAM Policies: Attach policies to users, groups, or roles to grant specific permissions.
● Bucket Policies: Define access control rules directly on the bucket.
● Permissions: Common permissions include:
● Read: Allows users to read objects from the bucket.
● Write: Allows users to write objects to the bucket.
● Delete: Allows users to delete objects from the bucket.
● List: Allows users to list objects in the bucket.
87. How can Instance 2, with a static IP, communicate with Instance 1, which is in a private subnet and
mapped to a multi-AZ load balancer?
Instance Communication with Private Subnet
● Method: Use a NAT gateway or a bastion host:
● NAT Gateway: A managed service that provides internet access for instances in a private
subnet.
● Bastion Host: A secure server in a public subnet that allows access to private instances.
88. For an EC2 instance in a private subnet, how can it verify and download required packages from the
internet without using a NAT gateway or bastion host? Are there any other AWS services that can
facilitate this?
EC2 Instance in Private Subnet Accessing Internet
● Method: Use a private DNS hostname:
1. Private DNS: Configure a private DNS zone within your VPC.
2. Private Hostname: Assign a private hostname to the EC2 instance.
3. DNS Resolution: The EC2 instance can resolve the private hostname to access internet
resources.
89. What is the typical latency for a load balancer, and if you encounter high latency, what monitoring
steps would you take?
Load Balancer Latency and Monitoring
● Typical Latency: Load balancer latency typically ranges from a few milliseconds to a few hundred
milliseconds, depending on factors like network conditions and load.
● Monitoring Steps:
● CloudWatch Metrics: Monitor load balancer latency using CloudWatch metrics.
● Network Tracing: Use tools like AWS X-Ray to trace requests through the load balancer
and identify bottlenecks.
● Performance Testing: Run load tests to simulate real-world traffic and identify
performance issues.
90. If your application is hosted in S3 and users are in different geographic locations, how can you reduce
latency?
Reducing Latency for S3 Hosted Application
● Methods:
● Edge Locations: Use AWS CloudFront to cache content at edge locations closer to users,
reducing latency.
● Regional Buckets: Store data in S3 buckets in the same region as the users, minimizing
network hops.
● Content Delivery Networks (CDNs): Use a CDN to distribute content across multiple
locations, reducing latency for users worldwide.
● Which services can be integrated with a CDN (Content Delivery Network)?
91.
Which services can be integrated with a CDN (Content Delivery Network)?
● CDN Integration
● Services: CDNs can integrate with various services, including:
● Web Servers: Apache, Nginx, IIS.
● Content Management Systems (CMS): WordPress, Drupal, Joomla.
● Cloud Storage: AWS S3, Google Cloud Storage, Azure Blob Storage.
● Streaming Services: Netflix, YouTube, Twitch.
● API Gateways: AWS API Gateway, Google Cloud Endpoints.
92.How do you dynamically retrieve VPC details from AWS to create an EC2 instance using IaC?
● . Dynamically Retrieving VPC Details
● Method: Use Terraform's data sources:
1. aws_vpc Data Source: Retrieve details of a specific VPC by its ID or name.
2. aws_subnet Data Source: Retrieve details of subnets within a VPC.
3. aws_security_group Data Source: Retrieve details of security groups associated with a
VPC.
4. aws_instance Data Source: Retrieve details of existing EC2 instances within a VPC.
93. Managing Unmanaged Resources in Terraform
● Approach: Use Terraform import command to bring existing unmanaged resources under
Terraform's control. This allows you to manage them alongside your IaC code.
94. Passing Arguments to VPC During Import
● Method: Use the --var flag with the terraform import command to pass arguments:
terraform import aws_vpc.example "vpc-1234567890abcdef0" --
var="cidr_block=10.0.0.0/16"
25. What is the master-slave architecture in Jenkins?
A: A master-slave architecture in Jenkins allows you to distribute build tasks across multiple nodes
(slaves). This provides:
● Parallel Execution: Run builds concurrently on multiple nodes, reducing build times.
● Resource Optimization: Utilize different hardware configurations for different build tasks.
● Scalability: Scale your Jenkins infrastructure by adding more slave nodes.
26. How do you integrate LDAP with AWS and Jenkins?
A: You can integrate LDAP with AWS and Jenkins by:
1. Configuring LDAP: Set up an LDAP server and configure it to authenticate users.
2. AWS IAM: Create an IAM role with permissions to access the LDAP server.
3. Jenkins Configuration: Configure Jenkins to use the LDAP server for authentication.
27. What are some key features of GitHub?
A: Key features of GitHub include:
● Version Control: Track changes to code over time.
● Collaboration: Facilitate collaboration among developers.
● Pull Requests: Enable code reviews and approvals.
● Issues: Track bugs, feature requests, and other tasks.
● Projects: Organize and manage work items.
28. What are some key features of Jenkins?
A: Key features of Jenkins include:
● Continuous Integration (CI): Automate build, test, and deployment processes.
● Continuous Delivery (CD): Deploy applications to different environments.
● Pipeline as Code: Define pipelines using code (Jenkinsfile).
● Plugins: Extend Jenkins functionality with a wide range of plugins.
● Master-Slave Architecture: Distribute build tasks across multiple nodes.
29. What are the benefits and uses of CI/CD?
A: Benefits of CI/CD:
● Faster Delivery: Reduce the time it takes to deliver new features and updates.
● Improved Quality: Catch bugs and errors early in the development process.
● Increased Efficiency: Automate repetitive tasks, freeing up developers to focus on innovation.
● Reduced Risk: Deploy changes more frequently and with less risk.
Uses of CI/CD:
● Software Development: Automate build, test, and deployment processes.
● Infrastructure Management: Provision and configure infrastructure automatically.
● Data Pipelines: Automate data processing and analysis tasks.
30. What is a GitHub workflow, and how is it used?
A: A GitHub workflow is a set of automated tasks that are triggered by events in a GitHub repository. You
use workflows to:
● Build and Test Code: Automate build and test processes.
● Deploy Applications: Deploy applications to different environments.
● Run Code Analysis: Perform code quality checks and security scans.
31. How do you handle merge conflicts in Git?
A: You handle merge conflicts in Git by:
1. Identifying Conflicts: Git will identify conflicts when merging branches.
2. Resolving Conflicts: Manually resolve conflicts by editing the affected files.
3. Staging Changes: Stage the resolved files using git add.
4. Committing Changes: Commit the changes with a descriptive message using git commit.
32. What steps do you take when a build fails in Jenkins?
A: When a build fails in Jenkins, you should:
1. Analyze Logs: Review the build logs to identify the cause of the failure.
2. Troubleshoot: Investigate the issue and try to resolve it.
3. Fix Code: If the failure is due to a code error, fix the code and rebuild.
4. Update Configuration: If the failure is due to a configuration issue, update the Jenkins
configuration.
5. Rollback: If necessary, roll back to a previous working version of the application.
17. How do you execute jobs in AWS?
A: You can execute jobs in AWS using:
● AWS Batch: A fully managed batch computing service for running large-scale, compute-intensive
jobs.
● AWS Lambda: A serverless computing service for running code in response to events.
● AWS ECS: A container orchestration service for deploying and managing containerized
applications.
18. What are Ansible roles, and how do you use them?
A: Ansible roles are a way to organize and reuse Ansible playbooks. They encapsulate tasks, variables,
and dependencies related to a specific component or service. You use roles to:
● Modularize Playbooks: Break down complex playbooks into smaller, reusable units.
● Simplify Deployment: Deploy multiple components or services with a single role.
● Improve Maintainability: Make it easier to manage and update Ansible configurations.
19. How do you ensure data persistence with Docker volumes?
A: Use Docker volumes to persist data outside the container:
● Named Volumes: Create named volumes that can be shared between containers.
● Data Volumes: Mount data volumes from the host machine into the container.
● Bind Mounts: Mount directories from the host machine into the container.
20. What are the key differences between Docker and Kubernetes?
A: Key differences:
● Scope: Docker focuses on containerization, while Kubernetes focuses on container orchestration.
● Management: Docker manages individual containers, while Kubernetes manages clusters of
containers.
● Scalability: Kubernetes provides advanced features for scaling and managing large-scale
deployments.
● Networking: Kubernetes offers more sophisticated networking capabilities for container
communication.
21. How do you securely store credentials in GitHub?
A: You can securely store credentials in GitHub using:
● GitHub Secrets: Use GitHub Secrets to store sensitive information securely.
● Environment Variables: Set environment variables in your GitHub workflow to access credentials.
● GitHub Actions: Use GitHub Actions to manage credentials and access them within your
workflow.
22. Where is the Jenkinsfile typically stored?
A: The Jenkinsfile is typically stored in the root directory of your Git repository.
23. How is pull request approval managed in GitHub?
A: GitHub provides features for managing pull request approvals:
● Required Approvers: Specify the number of reviewers required to approve a pull request.
● Review Policies: Define policies for code reviews, such as requiring specific reviewers or checks.
● Approval Flow: Control the approval process, such as requiring approvals from specific teams or
roles.
24. How do you execute a shell script within a Python script?
A: Use the subprocess module:
import subprocess
subprocess.run(["/path/to/script.sh"])
19. How do you ensure data persistence with Docker volumes?
A: Use Docker volumes to persist data outside the container:
● Named Volumes: Create named volumes that can be shared between containers.
● Data Volumes: Mount data volumes from the host machine into the container.
● Bind Mounts: Mount directories from the host machine into the container.
20. What are the key differences between Docker and Kubernetes?
A: Key differences:
● Scope: Docker focuses on containerization, while Kubernetes focuses on container orchestration.
● Management: Docker manages individual containers, while Kubernetes manages clusters of
containers.
● Scalability: Kubernetes provides advanced features for scaling and managing large-scale
deployments.
● Networking: Kubernetes offers more sophisticated networking capabilities for container
communication.
21. How do you securely store credentials in GitHub?
A: You can securely store credentials in GitHub using:
● GitHub Secrets: Use GitHub Secrets to store sensitive information securely.
● Environment Variables: Set environment variables in your GitHub workflow to access credentials.
● GitHub Actions: Use GitHub Actions to manage credentials and access them within your
workflow.
22. Where is the Jenkinsfile typically stored?
A: The Jenkinsfile is typically stored in the root directory of your Git repository.
23. How is pull request approval managed in GitHub?
A: GitHub provides features for managing pull request approvals:
● Required Approvers: Specify the number of reviewers required to approve a pull request.
● Review Policies: Define policies for code reviews, such as requiring specific reviewers or checks.
● Approval Flow: Control the approval process, such as requiring approvals from specific teams or
roles.
7. Have you upgraded any Kubernetes clusters?
A: (This is a yes/no question, followed by a description of your experience if you have.)
8. How do you deploy an application in a Kubernetes cluster?
A: You can deploy applications in a Kubernetes cluster using:
● kubectl: Use kubectl commands to deploy applications using YAML or JSON manifests.
● Helm: Use Helm charts to package and deploy applications, simplifying the process.
● Knative: Use Knative for serverless deployments on Kubernetes.
9. How do you communicate with a Jenkins server and a Kubernetes cluster?
A: You can communicate with a Jenkins server and a Kubernetes cluster using:
● Jenkins Plugins: Use Jenkins plugins like the Kubernetes plugin to interact with a Kubernetes
cluster.
● API Calls: Use the Kubernetes API to interact with the cluster programmatically.
● kubectl: Use kubectl commands from within Jenkins to manage Kubernetes resources.
10. How do you generate Kubernetes cluster credentials?
A: You can generate Kubernetes cluster credentials using:
● Service Accounts: Create service accounts in Kubernetes to provide access to specific
resources.
● kubeconfig: Generate a kubeconfig file that contains authentication and connection details for
the cluster.
11. Do you only update Docker images in Kubernetes, or do you also update replicas, storage levels, and
CPU allocation?
A: You can update various Kubernetes resources, including:
● Docker Images: Update the container image used by a deployment.
● Replicas: Scale up or down the number of replicas for a deployment.
● Storage Levels: Adjust storage capacity for persistent volumes.
● CPU Allocation: Modify CPU and memory resource requests and limits for containers.
12. What types of pipelines are there in Jenkins?
A: Jenkins offers several pipeline types:
● Pipeline: A flexible and powerful way to define complex workflows.
● Freestyle: A simpler option for basic build and deployment tasks.
● Multibranch Pipeline: Automatically creates pipelines for different branches in a Git repository.
13. Can you define environment variables inside your Jenkins pipeline?
A: Yes, you can define environment variables inside your Jenkins pipeline using:
● Pipeline Script: Use the environment directive within your Jenkinsfile to define environment
variables.
● Global Variables: Configure global environment variables in Jenkins settings.
● Credentials: Store sensitive information as credentials and access them within the pipeline.
14. What is the role of artifacts in Jenkins, and why do we need to push them to Nexus instead of
building and storing them locally?
A: Artifacts are the output of a build process, such as compiled code, container images, or test reports.
Pushing artifacts to Nexus (a repository manager) provides:
● Version Control: Track different versions of artifacts.
● Dependency Management: Manage dependencies between projects.
● Security: Control access to artifacts and ensure their integrity.
15. If you’re developing a Python-based application, how do you separate the packages needed for your
local deployment to avoid interfering with globally installed packages?
A: Use virtual environments:
1. Create Virtual Environment: Use python -m venv <env_name> to create a virtual
environment.
2. Activate Environment: Activate the environment using source <env_name>/bin/activate.
3. Install Packages: Install packages specific to your project using pip install
<package_name>.
3. What are the prerequisites before importing a VPC in Terraform?
A: Before importing a VPC, you need:
● Terraform Configuration: A Terraform configuration file defining the VPC resource you want to
import.
● Resource ID: The unique identifier (ID) of the VPC in AWS.
● Resource Type: The correct Terraform resource type (e.g., aws_vpc).
4. If an S3 bucket was created through Terraform but someone manually added a policy to it, how do
you handle this situation using IaC?
A: You can use Terraform's terraform plan command to detect differences between the current state of
the S3 bucket and your IaC configuration. This will highlight the manually added policy. You can then:
● Update IaC: Modify your Terraform configuration to include the manually added policy, ensuring
consistency.
● Ignore: If the policy is acceptable, use the ignore_changes option in Terraform to exclude it from
future plans.
5. How do you handle credentials for a PHP application accessing MySQL or any other secrets in
Docker?
A: You can manage credentials securely in Docker using:
● Environment Variables: Set environment variables within the Docker container to store
credentials.
● Secrets Management: Use a secrets manager like AWS Secrets Manager or HashiCorp Vault to
store and retrieve credentials securely.
● Docker Secrets: Use Docker secrets to store sensitive information separately from the container
image.
6. What is the command for running container logs?
A: The command for running container logs is:
docker logs containerid or container name
-----------------------------------------------------------------------------

[22:44, 02/09/2024] sagar magar: Here’s a comprehensive list of answers to the interview questions based on the topics you've mentioned:

### *LTIMintree 1st*

1. *Build Tools Used:*
   - Common build tools include Maven, Gradle, Ant (for Java), npm (for Node.js), and Make (for various languages).

2. *Port Number of Ansible:*
   - Ansible does not use a specific port; it uses SSH (typically port 22) to connect to target hosts by default.

3. *Webhook vs PollSCM:*
   - *Webhook:* Triggers builds or actions in Jenkins (or other CI/CD tools) based on events from a source repository (e.g., GitHub webhook sends a request when code is pushed).
   - *PollSCM:* Jenkins periodically checks the source code repository for changes and triggers builds if changes are detected.

4. *Difference between IAM User, IAM Role, and Policy:*
   - *IAM User:* An entity representing a person or application that interacts with AWS resources.
   - *IAM Role:* An entity that defines a set of permissions for making AWS service requests. It is assumed by IAM users or AWS services.
   - *IAM Policy:* A document that defines permissions (e.g., read, write) for IAM users, roles, or groups.

5. *Deployment to Kubernetes for Source Code:*
   - Build Docker images from source code.
   - Push the images to a container registry.
   - Create Kubernetes manifests (e.g., Deployment, Service).
   - Apply the manifests using kubectl apply.

6. *Pipeline Triggering on Code Changes:*
   - *Webhooks*: For Git-based SCMs like GitHub or GitLab, webhooks notify Jenkins of changes.
   - *Polling SCM*: Jenkins periodically polls the repository for changes.

7. *Ansible Tower:*
   - Ansible Tower is the enterprise version of Ansible that provides a web-based interface, role-based access control, and other advanced features.

8. *Ansible Master:*
   - Ansible Master refers to the central node where Ansible is installed and run. It manages playbooks, inventory, and configurations.

9. *Ingress Controller:*
   - An Ingress Controller manages access to services in a Kubernetes cluster by implementing rules defined in Ingress resources. It handles external access to services.

10. *Connecting 50 VPCs to the Internet:*
    - Use *NAT Gateways* or *NAT Instances* in each VPC or use *Transit Gateway* for centralized routing and internet access management.

### *LTIMintree 2nd*

1. *Adding SSL Certificate to S3 Static Website:*
   - Use Amazon *CloudFront* to front your S3 bucket, and configure an SSL certificate in CloudFront.

2. *Patching Kubernetes POD:*
   - Use rolling updates or recreate the pod with updated configuration. For manual patches, you might update the container image or configuration and apply changes using kubectl apply.

3. *Ticketing Tool Used:*
   - Common ticketing tools include Jira, ServiceNow, or Zendesk.

4. *Docker Networking:*
   - Types include *bridge, **host, **overlay, and **macvlan* networks.

5. *Docker Compose:*
   - *Docker Compose* is a tool for defining and running multi-container Docker applications using a docker-compose.yml file.

6. *Checking Docker Image Details:*
   - Use docker inspect <image-name> to get details about the image, including the base image and configuration.

7. *Moving Files in S3 After 30 Days:*
   - Use *S3 Lifecycle Policies* to transition objects to a different storage class or delete them after a specified period.

8. *Granting EC2 Instance Access to S3 Bucket:*
   - Attach an *IAM Role* to the EC2 instance with policies granting the required S3 permissions.

9. *Tracking Deleted Instances in AWS Console:*
   - Use *AWS CloudTrail* to log and monitor API calls related to EC2 instances.

10. *Handling Terraform Changes:*
    - Use terraform plan to preview changes before applying. Manage manual changes with terraform import if necessary.

11. *Terraform Destroy Dependency Error:*
    - Resolve dependencies by manually removing or modifying dependent resources or adjust the terraform configuration.

12. *Deployment in Kubernetes:*
    - A Deployment manages a set of replicas of a pod, ensuring that the desired number of pod replicas are running and available.

13. *Terraform Version and Upgrade:*
    - Check version with terraform version. Upgrade by downloading the new version and replacing the old binary.

14. *Upgrading Process Role:*
    - Describe your specific role, such as planning, testing, or executing the upgrade.

15. *Creating Terraform Modules:*
    - Define modules for VPC, subnets, and instances in separate files and call them in the desired order.

16. *Banking Application Architecture:*
    - Design should include *multiple availability zones, **highly available database, **load balancing, **secure communication* (e.g., SSL), *auto-scaling, and **disaster recovery*.

17. *AWS Lambda:*
    - AWS Lambda is a serverless compute service that automatically scales and manages servers. You only pay for the compute time you consume.

18. *NACL vs SG:*
    - *Network Access Control Lists (NACLs):* Operate at the subnet level, allow or deny inbound and outbound traffic.
    - *Security Groups (SGs):* Operate at the instance level, act as a virtual firewall to control inbound and outbound traffic.

### *Talentica 1st*

1. *Create 3 VPCs and Pair Them:*
   - Define VPCs with CIDR blocks and subnets. Use VPC Peering or Transit Gateway to connect them across accounts.

2. *Dockerfile for Java Application:*
   dockerfile
   FROM openjdk:11-jre-slim
   COPY target/my-app.jar /usr/src/myapp/my-app.jar
   WORKDIR /usr/src/myapp
   CMD ["java", "-jar", "my-app.jar"]
   

3. *POD vs Container:*
   - *POD:* A Kubernetes abstraction that can contain one or more containers with shared storage and networking.
   - *Container:* A lightweight, standalone, and executable software package that includes everything needed to run an application.

4. *Multiple Containers in a Single POD:*
   - Yes, a single POD can contain multiple containers that share the same network namespace and storage.

5. *Docker Networking:*
   - Types include *bridge, **host, **overlay, **macvlan, and **none*.

6. *Delete POD in Different Namespace:*
   sh
   kubectl delete pod <pod-name> -n <namespace>
   

7. *Delete Namespace:*
   sh
   kubectl delete namespace <namespace>
   

8. *CloudFront and Alternative to CDN:*
   - *CloudFront:* AWS’s CDN service for caching content.
   - *Alternative:* Directly serve content from S3 or use a custom solution.

9. *CloudFormation:*
   - AWS CloudFormation is a service for defining and provisioning infrastructure as code using JSON or YAML templates.

10. *RBAC and POD:*
    - *Role-Based Access Control (RBAC):* Manages permissions in Kubernetes. Use Roles or ClusterRoles to define permissions and RoleBindings or ClusterRoleBindings to assign them.

11. *Terraform IAM Resource for EC2:*
    hcl
    resource "aws_iam_role" "example" {
      name = "example-role"
      assume_role_policy = jsonencode({
        Version = "2012-10-17"
        Statement = [
          {
            Action = "sts:AssumeRole"
            Effect = "Allow"
            Principal = {
              Service = "ec2.amazonaws.com"
            }
          },
        ]
      })
    }

    resource "aws_iam_role_policy_attachment" "example" {
      role       = aws_iam_role.example.name
      policy_arn = "arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess"
    }
    

12. *Best Practices for Terraform State File:*
    - Use remote backends (e.g., AWS S3 with state locking using DynamoDB).
    - Encrypt the state file.
    - Use versioning and keep backups.

13. *Terraform EC2 Module:*
    - If only AMI ID and instance type are specified, the EC2 instance will be created in the default VPC or subnet and use the default security group unless otherwise specified.

14. *Recover Lost Terraform State File and Key Pair:*
    - *State File:* Restore from remote backup or use a state recovery tool.
    - *Key Pair:* Recreate the key pair and update instances with new credentials.

15. *Deploy App to Kubernetes:*
    - Build Docker images, push to a registry, create Kubernetes manifests (Deployment, Service), and apply using kubectl apply.

### *TransUnion*

1. *Branching Strategies:*
   - Common strategies include *Git Flow, **Feature Branch Workflow, and **GitHub Flow*.

2. *Git Stash:*
   - Temporarily shelves changes in your working directory and re-applies them later.

3. *Check Logs in GitHub:*
   - View commit history and pull request logs from the GitHub repository interface.

4. *Merge Changes from Main Branch:*
   - Use git merge main or git rebase main to integrate changes from the main branch into your feature branch.

5. *Git Sync vs Git Push:*
   -
[22:46, 02/09/2024] sagar magar: ### *TransUnion Follow-up Questions*

1. *How can we push only a particular commit to the Git repository?*
   - You can cherry-pick a particular commit and push it to the remote repository using the following steps:
     sh
     git checkout <target-branch>  # Switch to the branch where you want to apply the commit
     git cherry-pick <commit-hash>  # Apply the specific commit to the target branch
     git push origin <target-branch>  # Push the changes to the remote repository
     

2. *What are the types of pipelines in Jenkins?*
   - *Declarative Pipeline:* A simpler, more structured syntax that defines the entire pipeline in a Jenkinsfile.
   - *Scripted Pipeline:* A more flexible and powerful pipeline, defined using Groovy scripts.

3. *Difference between Declarative and Scripted Pipeline:*
   - *Declarative Pipeline:*
     - Structured, easy to read and maintain.
     - Uses a pipeline block and provides predefined syntax.
     - Example:
       groovy
       pipeline {
         agent any
         stages {
           stage('Build') {
             steps {
               sh 'make build'
             }
           }
         }
       }
       
   - *Scripted Pipeline:*
     - More flexible, allows the use of full Groovy syntax.
     - Suitable for complex pipelines.
     - Example:
       groovy
       node {
         stage('Build') {
           sh 'make build'
         }
       }
       

4. *How do you pass variables in a pipeline?*
   - Variables can be passed in Jenkins pipelines using the environment block for Declarative pipelines or by directly defining them in Scripted pipelines.
     - *Declarative Example:*
       groovy
       pipeline {
         environment {
           MY_VAR = "value"
         }
         stages {
           stage('Build') {
             steps {
               echo "Variable value is ${MY_VAR}"
             }
           }
         }
       }
       
     - *Scripted Example:*
       groovy
       node {
         def MY_VAR = "value"
         stage('Build') {
           echo "Variable value is ${MY_VAR}"
         }
       }
       

5. *In which language are scripted pipelines written?*
   - Scripted pipelines in Jenkins are written in *Groovy*.

6. *How to manage dynamic inventory in Ansible?*
   - Use a *dynamic inventory script* or *inventory plugins* that can fetch inventory data from external sources (like AWS, GCP, etc.) at runtime. For example, using AWS:
     sh
     ansible-inventory -i aws_ec2.yml --list
     
   - Dynamic inventory plugins like aws_ec2, azure_rm, gcp_compute, etc., are commonly used.

7. *Which module is used to process hundreds of variables in Ansible?*
   - The include_vars or vars_files modules are used to load multiple variables from files.
     yaml
     - name: Load variables from multiple files
       include_vars:
         dir: /path/to/vars/
         extensions: ["yml", "yaml"]
     

8. *How do you encrypt secrets and use them in an Ansible module?*
   - Use *Ansible Vault* to encrypt secrets:
     sh
     ansible-vault encrypt secrets.yml
     
   - To use the encrypted secrets in a playbook:
     yaml
     - hosts: all
       vars_files:
         - secrets.yml
       tasks:
         - name: Use secret variable
           debug:
             msg: "The secret value is {{ secret_var }}"
     

9. *Write the module to change the port from 80 to 8080:*
   - Here’s an example using Ansible to update a configuration file:
     yaml
     - name: Change port from 80 to 8080
       lineinfile:
         path: /etc/myapp/config.conf
         regexp: '^port=80'
         line: 'port=8080'
         backup: yes
     

10. *How to take backup and restore in ETCD:*
    - *Backup:*
      sh
      ETCDCTL_API=3 etcdctl snapshot save /path/to/backup.db --endpoints=<endpoints> --cacert=<ca.crt> --cert=<client.crt> --key=<client.key>
      
    - *Restore:*
      sh
      ETCDCTL_API=3 etcdctl snapshot restore /path/to/backup.db --data-dir /var/lib/etcd --name <name> --initial-cluster <cluster> --initial-advertise-peer-urls <url>
      

11. *Which port does the ETCD database run on?*
    - ETCD typically runs on *port 2379* (client communication) and *port 2380* (peer communication).

12. *What are the types of controllers in Kubernetes and their roles?*
    - *Deployment Controller:* Manages Deployment objects, ensuring the desired number of replicas are running.
    - *ReplicaSet Controller:* Ensures that a specified number of pod replicas are running at any given time.
    - *StatefulSet Controller:* Manages stateful applications, maintaining stable, unique network identifiers for pods.
    - *DaemonSet Controller:* Ensures that a copy of a pod runs on all (or some) nodes in the cluster.
    - *Job Controller:* Manages job objects that run a pod or pods to completion.
    - *CronJob Controller:* Manages cron job objects, running jobs on a schedule.

13. *How do you manage secrets in Kubernetes?*
    - Use Kubernetes *Secrets* objects:
      yaml
      apiVersion: v1
      kind: Secret
      metadata:
        name: my-secret
      type: Opaque
      data:
        username: c3RhY2thYm94
        password: cGFzc3dvcmQ=
      
    - Reference the secret in a pod:
      yaml
      apiVersion: v1
      kind: Pod
      metadata:
        name: mypod
      spec:
        containers:
        - name: mycontainer
          image: myimage
          env:
          - name: USERNAME
            valueFrom:
              secretKeyRef:
                name: my-secret
                key: username
          - name: PASSWORD
            valueFrom:
              secretKeyRef:
                name: my-secret
                key: password
      

14. *Write a deployment file to deploy 5 pods in a Kubernetes cluster:*
    yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my-deployment
    spec:
      replicas: 5
      selector:
        matchLabels:
          app: myapp
      template:
        metadata:
          labels:
            app: myapp
        spec:
          containers:
          - name: my-container
            image: my-image
            ports:
            - containerPort: 80
    

These answers should help you in preparing for your interviews and cover a wide range of DevOps and Kubernetes-related topics.
[22:47, 02/09/2024] sagar magar: The terms "git sync" and "git push" refer to different concepts and commands in the context of Git. Here's the difference between them:

### *git push:*
- *Definition*: The git push command is used to upload (or "push") your local repository changes to a remote repository, such as GitHub or GitLab.
- *Usage*: It transfers the commits from your local branch to the corresponding branch in the remote repository.
- *Example*:
  sh
  git push origin main
  
  This command pushes your changes in the main branch to the remote repository named origin.

### *git sync:*
- *Definition*: git sync is not a native Git command. However, in some workflows or custom scripts, git sync might refer to a sequence of commands that synchronizes your local branch with the remote branch, typically by pulling the latest changes from the remote and then pushing your local changes.
- *Usage*: It can be a combination of git pull (to fetch and merge changes from the remote repository) and git push (to push your local changes).
- *Example*:
  sh
  git pull origin main
  git push origin main
  
  This sequence first pulls the latest changes from the main branch on the remote repository and then pushes your local changes to it.

### *Summary*:
- *git push* is an official Git command used to upload local changes to a remote repository.
- *git sync* is not an official Git command but may be used in some contexts to refer to the process of ensuring your local branch is in sync with the remote branch, often involving both git pull and git push.
[09:08, 03/09/2024] sagar magar: ### LTIMintree 1st Interview

*1. What are the build tools you used?*
   - *Answer:* The build tools I have used include *Maven, **Gradle, and **Ant* for building Java applications. These tools help in compiling source code, packaging the code into executables, and managing dependencies.

*2. What is the port number of Ansible?*
   - *Answer:* Ansible typically uses *port 22*, which is the default port for SSH, as it relies on SSH to communicate with managed nodes.

*3. What are Webhook and PollSCM?*
   - *Answer:* 
     - *Webhook:* A webhook is a method in which one system can notify another system in real-time when a particular event occurs. In Jenkins, webhooks are commonly used to trigger builds automatically when changes are pushed to a repository.
     - *PollSCM:* PollSCM is a Jenkins job trigger mechanism where Jenkins checks the source code repository at regular intervals to see if there have been any changes. If changes are detected, the job is triggered.

*4. Difference between IAM User, IAM Role, and IAM Policy?*
   - *Answer:*
     - *IAM User:* An IAM User is an entity created in AWS that represents a person or application. It has permanent long-term credentials such as an access key and secret key.
     - *IAM Role:* An IAM Role is an AWS identity with permissions policies that determine what the identity can and cannot do in AWS. Unlike users, roles do not have long-term credentials but use temporary security credentials.
     - *IAM Policy:* An IAM Policy is a JSON document that defines permissions. Policies can be attached to users, groups, or roles to specify what actions are allowed or denied.

*5. What is the process to deploy source code to Kubernetes?*
   - *Answer:* 
     1. *Build the Application:* Compile the source code into a Docker image.
     2. *Push the Image:* Push the Docker image to a container registry like Docker Hub or Amazon ECR.
     3. *Create Kubernetes Manifests:* Write YAML files defining Kubernetes resources like Deployments, Services, and ConfigMaps.
     4. *Apply Manifests:* Use kubectl apply -f to deploy the resources to the Kubernetes cluster.
     5. *Monitor the Deployment:* Monitor the deployment using kubectl get pods and logs.

*6. How do pipelines get triggered if someone makes changes in the code?*
   - *Answer:* Pipelines are triggered using *webhooks* configured in the source code repository (e.g., GitHub, GitLab). When a change is pushed to the repository, the webhook sends a POST request to the CI/CD tool (like Jenkins), which triggers the pipeline.

*7. What is Ansible Tower?*
   - *Answer:* *Ansible Tower* is an enterprise framework by Red Hat for controlling, securing, and managing your Ansible automation with a UI, REST API, and role-based access control. It’s a centralized solution to run Ansible tasks and playbooks.

*8. What is Ansible Master?*
   - *Answer:* In Ansible, the master (often called the control node) is the machine where Ansible is installed. It controls and orchestrates the playbooks and commands that are executed on remote nodes (managed hosts).

*9. What is an Ingress Controller?*
   - *Answer:* An *Ingress Controller* is a Kubernetes resource that manages external access to services in a cluster, typically HTTP. It acts as a reverse proxy and can provide load balancing, SSL termination, and name-based virtual hosting.

*10. We have 50 VPCs that need to connect to the internet.*
   - *Answer:* To connect multiple VPCs to the internet, you can:
     - Create an *Internet Gateway* in each VPC and attach it to the VPC.
     - Set up *NAT Gateways* in public subnets for instances in private subnets to access the internet.
     - Use *VPC Peering* or *Transit Gateway* to interconnect VPCs if communication between them is needed.

### LTIMintree 2nd Interview

*1. If you hosted a static website using an S3 bucket, how can you add an SSL certificate?*
   - *Answer:* To add an SSL certificate to a static website hosted on S3, you can:
     1. Create a CloudFront distribution for the S3 bucket.
     2. Request an SSL certificate using AWS Certificate Manager (ACM).
     3. Associate the SSL certificate with the CloudFront distribution.
     4. Update the DNS settings in Route 53 to point to the CloudFront distribution.

*2. How can you perform patching of Kubernetes Pods?*
   - *Answer:* Patching Kubernetes pods can be done by:
     - *Rolling Update:* Update the container image in the Deployment manifest and apply it using kubectl apply -f, which triggers a rolling update.
     - *kubectl patch:* Use kubectl patch command to update specific fields in the pod spec.
     - *Recreate:* Delete the pods, and new ones with updated configurations will be automatically created by the Deployment.

*3. Which ticketing tool are you using?*
   - *Answer:* The commonly used ticketing tools include *Jira, **ServiceNow, and **Trello*.

*4. What are the Docker networking modes?*
   - *Answer:* Docker supports several networking modes, including:
     - *Bridge:* The default network mode, where each container gets its IP address.
     - *Host:* The container shares the host's network stack.
     - *Overlay:* For multi-host Docker networks, it allows containers on different hosts to communicate.
     - *None:* Disables networking for the container.

*5. What is Docker Compose and why is it used?*
   - *Answer:* *Docker Compose* is a tool for defining and running multi-container Docker applications. It uses a YAML file to define services, networks, and volumes. It’s used to manage complex applications with multiple containers as a single unit.

*6. How can you check the details of a Docker image, like which base AMI is used and other configurations?*
   - *Answer:* You can use docker inspect <image> to retrieve detailed information about a Docker image, including the base image, configuration options, and environment variables.

*7. If you are storing some data in an S3 bucket and want to move the file if not used in 30 days to save cost, how will you do that?*
   - *Answer:* You can create an *S3 Lifecycle Policy* to automatically move objects to a cheaper storage class like *S3 Glacier* or delete them if they have not been accessed for 30 days.

*8. You have one EC2 instance, and you want to give read and write permissions to an S3 bucket. How will you do that?*
   - *Answer:* 
     1. Create an *IAM Role* with a policy that grants s3:GetObject and s3:PutObject permissions.
     2. Attach the IAM Role to the EC2 instance.
     3. The EC2 instance will now have the necessary permissions to read from and write to the S3 bucket.

*9. If someone logged into the AWS console and deleted some instances, how can you track that activity?*
   - *Answer:* You can use *AWS CloudTrail* to track and log all API calls, including those made in the AWS Management Console. The logs can be searched to find out who deleted the instances and when.

*10. If someone made changes in resources created with Terraform and wants those changes, how can you take care while Terraform reapply?*
   - *Answer:* If manual changes are made to resources managed by Terraform, you should run terraform plan before terraform apply to review changes. You may need to use terraform import to bring existing resources into Terraform state.

*11. If you are deleting resources with terraform destroy but got an error that the resource can't be deleted because it has a dependency.*
   - *Answer:* Identify the dependency and either delete the dependent resource first or modify the Terraform configuration to handle the dependency correctly. Sometimes, manual intervention might be required to delete dependencies.

*12. What is deployment in Kubernetes?*
   - *Answer:* A *Deployment* in Kubernetes manages a set of identical pods, ensuring that the desired number of them are running at any given time. It’s used for updating applications, scaling them, and providing fault tolerance.

*13. Which version of Terraform are you using, and how to upgrade it?*
   - *Answer:* The Terraform version can be checked using terraform -v. To upgrade Terraform:
     - Download the new version from the official website.
     - Replace the old binary with the new one.
     - Run terraform init -upgrade to upgrade the Terraform providers.

*14. Are you part of any upgrading process, and what was your role in that?*
   - *Answer:* My role typically involves planning the upgrade, ensuring compatibility, testing in a staging environment, and then executing the upgrade in production with a rollback plan in place.

*15. How can you create a module in Terraform in order, like 1st VPC, then subnet, and then instance?*
   - *Answer:* In Terraform, dependencies between resources can be managed using implicit or explicit dependencies (e.g., using depends_on). Modules can be organized and called in the correct order within a root module.

*16. You got a requirement to create the architecture of a banking application that needs to be secure, highly available, and fault-tolerant.*
   -

 *Answer:* 
     - *Security:* Use IAM roles, security groups, and NACLs for access control. Implement encryption for data at rest and in transit.
     - *High Availability:* Deploy applications across multiple availability zones and use load balancers.
     - *Fault Tolerance:* Implement backups, replication, and auto-scaling. Use managed services like RDS with multi-AZ deployment.

*17. What is AWS Lambda?*
   - *Answer:* *AWS Lambda* is a serverless compute service that lets you run code without provisioning or managing servers. You only pay for the compute time you consume.

*18. NACL vs SG*
   - *Answer:*
     - *Network ACL (NACL):* Operates at the subnet level, is stateless, and allows or denies traffic based on rules.
     - *Security Group (SG):* Operates at the instance level, is stateful, and allows traffic to or from an instance based on rules.

### Talentica 1st Interview

*1. Create 3 VPCs in 2 different AWS accounts and peer them.*
   - *Answer:* 
     1. Create VPCs in each account with unique CIDR blocks.
     2. Set up VPC Peering between the VPCs in the two accounts.
     3. Update the route tables to allow traffic between the VPCs.

*2. Write a Dockerfile for a Java application.*
   - *Answer:* 
   Dockerfile
   FROM openjdk:11-jdk-slim
   WORKDIR /app
   COPY . /app
   RUN ./gradlew build
   CMD ["java", "-jar", "build/libs/app.jar"]
   

*3. What is a POD and how is it different from a container?*
   - *Answer:* A *Pod* is the smallest deployable unit in Kubernetes and can contain one or more containers. Containers within a pod share the same network namespace and storage.

*4. Can we create multiple containers in a single POD?*
   - *Answer:* Yes, multiple containers can be created within a single pod. These containers typically work closely together, sharing the same network namespace and storage.

*5. What are the Docker networking modes available?*
   - *Answer:* The networking modes include *Bridge, **Host, **Overlay, **None, and **Macvlan*.

*6. How to delete a pod in Kubernetes in a different namespace?*
   - *Answer:* Use kubectl delete pod <pod-name> -n <namespace>.

*7. How to delete a namespace?*
   - *Answer:* Use kubectl delete namespace <namespace-name>.

*8. What is CloudFront, and if I don't want to use CDN, what's the way to serve the requests?*
   - *Answer:* *CloudFront* is a Content Delivery Network (CDN) service by AWS. If you don’t want to use a CDN, you can serve content directly from an S3 bucket or an EC2 instance using a custom domain name with Route 53.

*9. What is CloudFormation?*
   - *Answer:* *AWS CloudFormation* is an infrastructure-as-code (IaC) service that lets you define AWS resources in JSON or YAML templates, enabling automated provisioning and management of resources.

*10. What is RBAC, and how to add it to a POD?*
   - *Answer:* *Role-Based Access Control (RBAC)* in Kubernetes controls who can perform what actions on which resources. You can bind a *Role* or *ClusterRole* to a *ServiceAccount* and use that ServiceAccount in a pod’s spec to enforce RBAC.

*11. Write a Terraform resource for IAM to provide read and write access to an EC2 instance.*
   - *Answer:* 
   hcl
   resource "aws_iam_role" "ec2_role" {
     name = "ec2_s3_access_role"
     assume_role_policy = jsonencode({
       Version = "2012-10-17"
       Statement = [
         {
           Action = "sts:AssumeRole"
           Effect = "Allow"
           Principal = {
             Service = "ec2.amazonaws.com"
           }
         }
       ]
     })
   }

   resource "aws_iam_policy" "s3_policy" {
     name = "ec2_s3_access_policy"
     policy = jsonencode({
       Version = "2012-10-17"
       Statement = [
         {
           Action = [
             "s3:GetObject",
             "s3:PutObject"
           ]
           Effect = "Allow"
           Resource = "arn:aws:s3:::your-bucket-name/*"
         }
       ]
     })
   }

   resource "aws_iam_role_policy_attachment" "attach_s3_policy" {
     role       = aws_iam_role.ec2_role.name
     policy_arn = aws_iam_policy.s3_policy.arn
   }

   resource "aws_instance" "my_instance" {
     ami           = "ami-12345678"
     instance_type = "t2.micro"
     iam_instance_profile = aws_iam_role.ec2_role.name
     ...
   }
   

*12. What are the best practices for Terraform state files?*
   - *Answer:* Best practices include:
     - Storing state files in a remote backend like S3 with DynamoDB for locking.
     - Enabling encryption for sensitive data.
     - Using versioning for state files.
     - Managing state file access with strict IAM policies.

*13. If we create an EC2 resource using a Terraform module by just providing AMI ID and instance type, in which subnet and security group will it be created?*
   - *Answer:* If not specified, the EC2 instance will be created in the default subnet of the default VPC, with the default security group attached.

*14. If we change the SG manually and reapply Terraform, what will happen?*
   - *Answer:* Terraform will detect the manual change as drift and attempt to revert the security group back to what is defined in the Terraform configuration.

*15. How to recover a lost Terraform state file and key pair of an EC2 instance?*
   - *Answer:* 
     - For a lost state file, use the remote backend's version history (if available) to restore it.
     - For a lost key pair, if the instance is already running, you can use EC2 Instance Connect, Systems Manager Session Manager, or attach a new key pair by creating an AMI and launching a new instance.

*16. Explain the process of deploying an app to a Kubernetes cluster from source code.*
   - *Answer:* 
     1. *Build the App:* Compile the source code into a Docker image.
     2. *Push to Registry:* Push the Docker image to a container registry.
     3. *Create Manifests:* Write Kubernetes YAML files for Deployments, Services, etc.
     4. *Deploy to Cluster:* Apply the manifests using kubectl.
     5. *Monitor:* Use kubectl get pods and other commands to ensure successful deployment.

### TransUnion Interview

*1. What are the branching strategies you used and how do they work?*
   - *Answer:* Common branching strategies include:
     - *GitFlow:* Uses separate branches for features, releases, and hotfixes. Development happens in the develop branch, and releases are merged into main.
     - *GitHub Flow:* A simpler strategy with a single main branch and feature branches.
     - *Trunk-based Development:* Developers commit to a single main branch with short-lived feature branches.

*2. What is Git stash?*
   - *Answer:* git stash is a command that temporarily saves your changes in a "stash" without committing them, allowing you to work on something else. You can later apply the stashed changes back to your working directory.

*3. How to check logs in GitHub?*
   - *Answer:* GitHub logs can be checked by viewing the actions logs in the GitHub Actions section for CI/CD pipelines, or by checking commit history and pull request logs.

*4. How can you merge changes from the main branch to a feature branch?*
   - *Answer:* You can merge changes from the main branch into your feature branch using git merge main while on the feature branch.

*5. Difference between git sync and git push.*
   - *Answer:* 
     - *git sync:* Typically refers to synchronizing your branch with the remote, pulling and pushing changes.
     - *git push:* Pushes your local changes to the remote repository.

*6. How can you push only a particular commit to the Git repo?*
   - *Answer:* You can use git cherry-pick <commit-hash> to apply a specific commit to a branch and then push that branch.

*7. What are the types of pipelines in Jenkins?*
   - *Answer:* The two main types of pipelines in Jenkins are:
     - *Declarative Pipeline:* A simpler, more opinionated way to define a pipeline.
     - *Scripted Pipeline:* A more flexible and powerful way to define a pipeline using Groovy code.

*8. Difference between Declarative and Scripted Pipeline?*
   - *Answer:* 
     - *Declarative Pipeline:* Easier to use, structured, and provides more built-in features and error handling.
     - *Scripted Pipeline:* More flexible, written in Groovy, and allows more complex logic.

*9. How do you pass variables in a pipeline?*
   - *Answer:* Variables can be passed using environment variables, parameters, or directly within the pipeline script.

**10. In which language are scripted

 pipelines written?**
   - *Answer:* Scripted pipelines in Jenkins are written in *Groovy*.

*11. How do you manage dynamic inventory in Ansible?*
   - *Answer:* Dynamic inventory in Ansible can be managed using scripts or plugins that fetch inventory data from external sources like AWS, GCP, or custom APIs.

*12. Which module is used to process 100s of variables in Ansible?*
   - *Answer:* The vars_files directive can be used to include multiple variable files in a playbook, and with_items can loop over items.

*13. How do you encrypt secrets and use them in Ansible modules?*
   - *Answer:* Use *Ansible Vault* to encrypt secrets and then reference them in playbooks by decrypting at runtime.

*14. Write a module to change the port from 80 to 8080.*
   - *Answer:* 
   yaml
   - name: Change port from 80 to 8080
     lineinfile:
       path: /etc/httpd/conf/httpd.conf
       regexp: '^Listen 80'
       line: 'Listen 8080'
       state: present
     notify: Restart httpd
   

*15. How do you take backup and restore in ETCD?*
   - *Answer:* 
     - *Backup:* Use etcdctl snapshot save <backup-file-path> to create a snapshot.
     - *Restore:* Use etcdctl snapshot restore <backup-file-path> and reconfigure the cluster with the restored data.

*16. On which port does the ETCD database run?*
   - *Answer:* *ETCD* typically runs on port *2379* for client communication and *2380* for peer communication.

*17. What are the types of controllers in Kubernetes and their roles?*
   - *Answer:* Types of controllers include:
     - *Deployment Controller:* Manages deployments and ensures the desired number of pod replicas.
     - *ReplicaSet Controller:* Ensures a specified number of pod replicas are running.
     - *DaemonSet Controller:* Ensures a copy of a pod runs on all or some nodes.
     - *StatefulSet Controller:* Manages stateful applications with stable, unique pod identities.

*18. How do you manage secrets in Kubernetes?*
   - *Answer:* Secrets in Kubernetes are managed using kubectl to create and manage secret objects, and then mounting them as files or environment variables in pods.

*19. Write a deployment file to deploy 5 pods in a Kubernetes cluster.*
   - *Answer:*
   yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: my-deployment
   spec:
     replicas: 5
     selector:
       matchLabels:
         app: my-app
     template:
       metadata:
         labels:
           app: my-app
       spec:
         containers:
         - name: my-container
           image: nginx
           ports:
           - containerPort: 80
   

This detailed answer format should provide a thorough understanding of the topics and help in preparation.
[10:48, 03/09/2024] sagar magar: servieces in k8s

ClusterIP: Default and most common, for internal communication within the cluster.

LoadBalancer: When you need to expose a service to the internet with a cloud provider’s load balancing.

NodePort: For direct external access to a service via a specific port on all nodes, often used in non-cloud environments or for testing.

ExternalName: When you want to alias an external DNS name inside your Kubernetes cluster.


Each service type in Kubernetes serves a different purpose, depending on whether the service should be internal, external, or need a special DNS configuration.
[11:11, 03/09/2024] sagar magar: The Master Node is the server (or set of servers) where the Control Plane components run.

The Control Plane is the set of processes that manage the Kubernetes cluster.


Control Plane:
Manages the overall cluster and its state, ensuring that the desired state of applications is maintained.

Worker Nodes:
Run the containerized applications, hosting the actual workload in the form of pods.

Core Components:
API Server: The central management point for all interactions with the cluster.

etcd: A distributed key-value store that holds the configuration and state of the cluster.

Controller Manager: Ensures the cluster’s desired state by managing various controllers (e.g., node, replication, and endpoint controllers).

Scheduler: Assigns pods to the most suitable nodes based on resource availability and constraints.

Kubelet: Ensures that containers in the pods are running and healthy on each worker node.

Kube-proxy: Manages network rules on nodes to enable communication between services and pods.

Container Runtime: Runs and manages the containers (e.g., Docker, containerd).

Networking and Storage:
Networking: Handled by CNI (Container Network Interface) plugins, enabling pod-to-pod communication and external access to services.

Storage: Provides persistent storage solutions through volumes, persistent volumes (PV), and persistent volume claims (PVC), allowing data to persist beyond the lifecycle of individual pods.
[11:15, 03/09/2024] sagar magar: The Control Plane is the set of processes that manage the Kubernetes cluster.
The Master Node is the server (or set of servers) where the Control Plane components run.
[12:08, 03/09/2024] sagar magar: AWS offers a variety of storage services tailored to different use cases. Here’s a summary of the primary types of storage options available in AWS:

### *1. Amazon S3 (Simple Storage Service)*
- *Type:* Object Storage
- *Use Case:* Ideal for storing and retrieving any amount of data at any time. Commonly used for backups, static website hosting, data archiving, and big data analytics.
- *Features:* Scalable, durable (99.999999999% durability), and offers various storage classes (Standard, Intelligent-Tiering, One Zone-IA, Glacier, Glacier Deep Archive).

### *2. Amazon EBS (Elastic Block Store)*
- *Type:* Block Storage
- *Use Case:* Provides persistent block storage for EC2 instances. Suitable for applications requiring a file system or database, such as boot volumes or data storage.
- *Features:* Includes several volume types (General Purpose SSD, Provisioned IOPS SSD, Throughput Optimized HDD, Cold HDD), and offers snapshot capabilities.

### *3. Amazon EFS (Elastic File System)*
- *Type:* File Storage
- *Use Case:* Managed file storage for use with AWS Cloud services and on-premises resources. Ideal for applications that need a shared file system across multiple instances.
- *Features:* Scalable, elastic, and supports NFS (Network File System) protocol.

### *4. Amazon FSx*
- *Type:* File Storage
- *Use Case:* Managed file systems for Windows and Lustre. 
  - *Amazon FSx for Windows File Server:* Fully managed Windows file system, compatible with Windows-based applications.
  - *Amazon FSx for Lustre:* High-performance file system for compute-intensive workloads such as machine learning and high-performance computing (HPC).
- *Features:* Native support for Windows NTFS and Lustre, and integrates with other AWS services.

### *5. Amazon Glacier and Glacier Deep Archive*
- *Type:* Archive Storage
- *Use Case:* Long-term data archiving and backup. Suitable for data that is rarely accessed but needs to be retained for long periods.
- *Features:* Low-cost storage with retrieval options ranging from minutes to hours (Glacier) or days (Deep Archive).

### *6. Amazon Storage Gateway*
- *Type:* Hybrid Storage
- *Use Case:* Integrates on-premises environments with cloud storage. It provides seamless and secure integration between on-premises IT environments and AWS storage services.
- *Features:* Supports file, volume, and tape gateways, enabling backup, archiving, and disaster recovery.

### *7. AWS Backup*
- *Type:* Backup Service
- *Use Case:* Centralized backup service for AWS services. Automates and centrally manages backups across AWS services like EBS, RDS, DynamoDB, EFS, and Storage Gateway.
- *Features:* Provides backup scheduling, retention management, and compliance reporting.

### *8. Amazon DynamoDB*
- *Type:* NoSQL Database
- *Use Case:* Fully managed NoSQL database service for high-performance applications requiring low-latency data access.
- *Features:* Offers automatic scaling, backup and restore, in-memory caching with DAX (DynamoDB Accelerator), and global tables for multi-region applications.

### *9. AWS Snowball*
- *Type:* Data Transfer and Edge Storage
- *Use Case:* Large-scale data transfer service using physical appliances to move data to and from AWS. Useful for large data migrations and edge computing.
- *Features:* Provides rugged, secure, and offline data transfer capabilities with encryption.

### *10. Amazon RDS (Relational Database Service)*
- *Type:* Managed Relational Database
- *Use Case:* Managed database service for relational databases such as MySQL, PostgreSQL, MariaDB, Oracle, and SQL Server.
- *Features:* Automated backups, scaling, patching, and replication.

### *11. Amazon Redshift*
- *Type:* Data Warehouse
- *Use Case:* Fully managed data warehouse service for large-scale data analytics and business intelligence.
- *Features:* Columnar storage, massively parallel processing (MPP), and integrates with various analytics tools.

### *Summary*

- *Object Storage:* Amazon S3
- *Block Storage:* Amazon EBS
- *File Storage:* Amazon EFS, Amazon FSx
- *Archive Storage:* Amazon Glacier, Glacier Deep Archive
- *Hybrid Storage:* Amazon Storage Gateway
- *Backup Service:* AWS Backup
- *NoSQL Database:* Amazon DynamoDB
- *Data Transfer and Edge Storage:* AWS Snowball
- *Managed Relational Database:* Amazon RDS
- *Data Warehouse:* Amazon Redshift

These storage services cater to different needs, from high-performance computing and data archiving to file sharing and database management.
[14:13, 03/09/2024] sagar magar: Certainly! Here’s a brief overview of Kubernetes service types:

1. *ClusterIP*: 
   - *Access:* Internal only.
   - *Use:* For communication between services within the cluster.

2. *NodePort*: 
   - *Access:* External via <NodeIP>:<NodePort>.
   - *Use:* Exposes services on a fixed port across all nodes for external access.

3. *LoadBalancer*: 
   - *Access:* External via a cloud provider’s load balancer IP.
   - *Use:* Provides a single IP address for accessing services from outside the cluster and handles traffic distribution.

4. *ExternalName*: 
   - *Access:* Internal, proxies to an external service via DNS.
   - *Use:* Maps a service to an external DNS name for integrating with services outside the cluster.

5. *Headless Service*: 
   - *Access:* Directly accesses pod IPs, no cluster IP.
   - *Use:* Useful for stateful applications and custom DNS configurations.

6. *Ingress* (not a service type but related):
   - *Access:* External, manages HTTP/HTTPS traffic.
   - *Use:* Routes external HTTP/HTTPS traffic to services inside the cluster, supports SSL termination and load balancing.
