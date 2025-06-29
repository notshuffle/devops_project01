
<<<<<<< HEAD
#  DevOps CI/CD Deployment – Docker • GitHub Actions • EC2

This project demonstrates how to take a simple Node.js web app and deploy it using Docker, GitHub Actions, and an EC2 instance behind NGINX—complete with automated CI/CD.
=======
```markdown
# 🚀 DevOps Project: Node.js App with Docker, GitHub Actions, and EC2 Deployment

This project showcases how to containerize a Node.js application, deploy it on an EC2 instance using NGINX as a reverse proxy, and automate the workflow via GitHub Actions.

---

## 🧰 Tech Stack

- **Node.js** – Application runtime
- **Docker** – Containerization
- **Docker Hub** – Image hosting
- **NGINX** – Reverse proxy server
- **AWS EC2** – Cloud VM for hosting
- **GitHub Actions** – CI/CD automation

---

## 📁 Project Structure

```
devops_project01/
├── Dockerfile
├── nginx.conf
├── server.js
├── package.json
├── .dockerignore
├── .gitignore
├── .github/
│   └── workflows/
│       └── ci-cd.yml
└── screenshots/
    └── ...deployment proof images
```
>>>>>>> c0d983b ( README.md, nginx.conf, and CI/CD pipeline)

---

## ✅ Setup Steps

### 1. Clone and Prepare the Code

```bash
git clone https://github.com/notshuffle/devops_project01.git
cd devops_project01
```

### 2. Dockerize and Push Image

```bash
docker build -t notshuffle/nodeapp:latest .
docker push notshuffle/nodeapp:latest
```

### 3. Launch EC2 Instance

- Open inbound rules: `22`, `80`, `5050`
- SSH into your instance and install Docker and NGINX:

```bash
sudo apt update && sudo apt install docker.io nginx -y
sudo usermod -aG docker ubuntu
```

- Transfer `nginx.conf` and reload NGINX:

```bash
scp -i devoops_project01.pem nginx.conf ubuntu@<EC2_PUBLIC_IP>:/home/ubuntu/
ssh -i devoops_project01.pem ubuntu@<EC2_PUBLIC_IP>
sudo mv /home/ubuntu/nginx.conf /etc/nginx/sites-available/default
sudo nginx -t && sudo systemctl reload nginx
```

### 4. Run Container on EC2

```bash
docker pull notshuffle/nodeapp:latest
docker run -d --name nodeapp -p 5050:5050 notshuffle/nodeapp:latest
```

---

## ⚙️ GitHub Actions Workflow

CI/CD is automated through `ci-cd.yml`:

- Runs on push to `main`
- Lints code using ESLint
- Builds and pushes Docker image
- Deploys to EC2 via SSH

### 🔑 Required GitHub Secrets

| Secret Name         | Description                                |
|---------------------|--------------------------------------------|
| `DOCKER_USERNAME`   | Your Docker Hub username                   |
| `DOCKER_PASSWORD`   | Your Docker Hub password/token             |
| `EC2_HOST`          | Public IP of EC2 instance                  |
| `EC2_USER`          | EC2 username (`ubuntu` for Ubuntu)         |
| `EC2_SSH_KEY`       | Private SSH key (PEM format as plain text) |

---

## 🧪 Local Development

```bash
npm install
npm start
```

<<<<<<< HEAD
---

=======
App runs locally on `http://localhost:5050`

---

## 📸 Screenshots

Include screenshots for:
- Docker Hub repository
- Running EC2 instance
- NGINX config and test output
- Successful GitHub Actions run
- App running in browser via EC2 public IP

---

## 🧑‍💻 Author

**Safal Silwal**  
GitHub: [@notshuffle](https://github.com/notshuffle)  
Docker Hub: [notshuffle/nodeapp](https://hub.docker.com/r/notshuffle/nodeapp)
```
>>>>>>> c0d983b ( README.md, nginx.conf, and CI/CD pipeline)

