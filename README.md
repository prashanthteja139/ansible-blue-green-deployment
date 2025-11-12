🌀 Ansible Blue-Green Deployment Automation
🧭 Overview

This repository contains an Ansible playbook that automates zero-downtime deployments using the Blue-Green strategy.
It ensures seamless transitions between application versions by routing traffic dynamically through Nginx, without affecting uptime or user sessions.

Blue-Green deployment is a DevOps best practice used by top tech companies (Amazon, Netflix, Spotify) to achieve continuous delivery with rollback safety.

⚙️ Key Features

✅ Automated deployment of web applications
✅ Dynamic Nginx configuration switch between Blue and Green environments
✅ Zero downtime during upgrades or releases
✅ Easy rollback mechanism in case of deployment failure
✅ Can integrate with CI/CD tools like Jenkins, GitHub Actions, or GitLab CI

🏗️ Project Structure
ansible-blue-green-deployment/
│
├── blue_green_deploy.yml        # Main Ansible playbook
├── nginx_blue_green.j2          # Nginx Jinja2 template for dynamic routing
└── README.md                    # Documentation

🔧 Variables
Variable	Description	Default
app_name	Application name	myapp
blue_env_path	Directory for Blue environment	/var/www/myapp-blue
green_env_path	Directory for Green environment	/var/www/myapp-green
repo_url	Git repository of your app	https://github.com/example/myapp.git
branch	Branch to deploy	main
active_env_file	Tracks currently active environment	/etc/nginx/conf.d/active_env.conf
nginx_config_path	Path to Nginx config	/etc/nginx/conf.d/default.conf
🚀 How It Works

Check Active Environment — Reads which environment (Blue or Green) is currently live.

Deploy Code — Pulls latest code from Git and installs dependencies in the inactive environment.

Build Application — Runs your build command (Node.js, Python, etc.).

Switch Traffic — Updates Nginx config to route traffic to the new environment.

Restart Nginx — Gracefully restarts the service without downtime.

Mark New Environment Active — Saves new active environment for future deployments.

📦 Usage



1️⃣ Clone the repository:
git clone https://github.com/<your-username>/ansible-blue-green-deployment.git
cd ansible-blue-green-deployment

2️⃣ Update your inventory file:
[webservers]
your_server_ip ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa

3️⃣ Customize variables:

Edit blue_green_deploy.yml and update:

repo_url

branch

app_name

4️⃣ Run the playbook:
ansible-playbook -i inventory.ini blue_green_deploy.yml

🔁 Rollback Strategy

If something goes wrong with the latest deployment:

Open /etc/nginx/conf.d/active_env.conf on the target server.

Change the environment manually:

echo "blue" | sudo tee /etc/nginx/conf.d/active_env.conf


Re-run the playbook — it will route traffic back to the previous version instantly.

🔐 Integration Ideas

CI/CD Pipelines: Trigger this playbook automatically after a successful build in Jenkins or GitHub Actions.

Ansible Vault: Store sensitive credentials securely.

Monitoring: Use Prometheus or Grafana to track deployment health and latency.

 Example CI/CD Integration
- name: Deploy Application
  uses: dawidd6/action-ansible-playbook@v2
  with:
    playbook: blue_green_deploy.yml
    inventory: inventory.ini
    key: ${{ secrets.SSH_KEY }}
    options: |
      -e "branch=release-v2"

Best Practices

Use Blue-Green deployment for production workloads.

Keep two stable environments ready at all times.

Always test on staging before promoting to production.

Integrate health checks to validate deployment before switching traffic.

🏁 Conclusion

This playbook demonstrates a practical, real-world DevOps automation workflow.
It ensures that your production stays live while enabling continuous updates — the true essence of Zero-Downtime CI/CD.
