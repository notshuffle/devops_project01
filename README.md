
#  DevOps CI/CD Deployment – Docker • GitHub Actions • EC2

This project demonstrates how to take a simple Node.js web app and deploy it using Docker, GitHub Actions, and an EC2 instance behind NGINX—complete with automated CI/CD.

---

##  What This Project Covers

- Cloning a starter project from GitHub
- Creating your own GitHub repo and pushing code
- Dockerizing the app and pushing it to Docker Hub
- Creating an EC2 instance and configuring security groups
- Installing Docker and NGINX on EC2
- Setting up NGINX as a reverse proxy (port 80 → 5050)
- Deploying the Docker container to EC2
- Writing a GitHub Actions CI/CD workflow
- Automating image builds and deployment via GitHub Secrets

---

## 🛠 Project Setup Steps

###  Clone and Prepare the Code

```bash
git clone https://github.com/ndangol16/dockerdemo.git
cp -r dockerdemo your-project-folder
cd your-project-folder
git init
git remote add origin git@github.com:<your-username>/<your-repo>.git
```

###  Dockerize the App

Create a `Dockerfile` and `.dockerignore`, then:

```bash
docker build -t yourusername/nodeapp:latest .
docker push yourusername/nodeapp:latest
```

###  Set Up EC2 and NGINX

1. Launch EC2 (Ubuntu)
2. Open inbound ports: 22, 80, and 5050
3. SSH into instance and install Docker + NGINX:
   ```bash
   sudo apt update
   sudo apt install docker.io nginx -y
   sudo usermod -aG docker ubuntu
   ```

4. Configure NGINX reverse proxy:
   ```
   location / {
     proxy_pass http://localhost:5050;
     proxy_http_version 1.1;
     proxy_set_header Upgrade $http_upgrade;
     proxy_set_header Connection 'upgrade';
     proxy_set_header Host $host;
     proxy_cache_bypass $http_upgrade;
   }
   ```

5. Restart NGINX: `sudo systemctl restart nginx`

6. Pull and run your Docker container:
   ```bash
   docker pull yourusername/nodeapp:latest
   docker run -d --name nodeapp -p 5050:5050 yourusername/nodeapp:latest
   ```

---

## GitHub Actions CI/CD

Create `.github/workflows/ci-cd.yml`:

```yaml
name: Deploy Node App

on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Log in to DockerHub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and Push Docker image
        run: |
          docker build -t ${{ secrets.DOCKERHUB_USERNAME }}/nodeapp:latest .
          docker push ${{ secrets.DOCKERHUB_USERNAME }}/nodeapp:latest

      - name: Deploy to EC2
        uses: appleboy/ssh-action@v1.0.0
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            docker pull ${{ secrets.DOCKERHUB_USERNAME }}/nodeapp:latest
            docker stop nodeapp || true
            docker rm nodeapp || true
            docker run -d --name nodeapp -p 5050:5050 ${{ secrets.DOCKERHUB_USERNAME }}/nodeapp:latest
```

---

##  GitHub Secrets to Set

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`
- `EC2_HOST`
- `EC2_USER` (e.g., `ubuntu`)
- `EC2_SSH_KEY` (private key contents in one line)

---

##  To Run Locally

```bash
npm install
npm start
```

---


