# CI/CD Pipeline using Jenkins, Docker & AWS EC2

A Jenkins pipeline that pulls a small Flask application from GitHub, builds a Docker image and runs it as a container on an AWS EC2 instance, exposing the app on port 80.

## Pipeline Flow

```
GitHub (main) ──▶ Jenkins ──▶ docker build ──▶ docker run (EC2) ──▶ http://<ec2-public-ip>
```

| Stage | What it does |
|---|---|
| Clone Repo | Pulls the `main` branch from this repository |
| Build Docker Image | `docker build -t flask-docker-app .` |
| Run Container | Removes any old container, then `docker run -d --name flask-docker-app -p 80:5000 flask-docker-app` (host port 80 → container port 5000) |

## Tech Stack

Jenkins · Docker · Python (Flask) · Git/GitHub · AWS EC2 (Ubuntu) · Linux

## Project Structure

```
.
├── app.py          # Flask app (returns "Hello from Flask")
├── dockerfile      # python:3.9 image, installs Flask, runs app.py
├── jenkinsfile     # declarative pipeline
└── README.md
```

## Prerequisites

- An EC2 instance (Ubuntu) with Jenkins and Docker installed
- The `jenkins` user added to the `docker` group so pipeline stages can run Docker commands
- Security group rules allowing port 8080 (Jenkins) and port 80 (the app)

## How to Run

1. Create a **Pipeline** job in Jenkins and choose *Pipeline script from SCM*, pointing to this repository and the `jenkinsfile`.
2. Click **Build Now**.
3. When the build succeeds, open `http://<ec2-public-ip>` in a browser. You should see **Hello from Flask**.

To run it locally without Jenkins:

```bash
docker build -t flask-docker-app .
docker run -d --name flask-docker-app -p 80:5000 flask-docker-app
```

## Screenshots

Add to a `screenshots/` folder:
- Jenkins stage view showing a successful build
- The app responding in the browser

## What I Learned

- Writing a declarative Jenkins pipeline
- Containerizing a Python application with a Dockerfile
- Running Jenkins and Docker on an EC2 instance, including permissions and security-group setup
- Troubleshooting failed builds from Jenkins console output

## Known Limitations / Next Steps

- Add a GitHub webhook so every push triggers a build automatically
- Push the image to Docker Hub or AWS ECR instead of building on the server
- Add a test stage and a notification step
- Recreate the pipeline in GitHub Actions

## Author

**Sneha Bodusu**: [GitHub](https://github.com/BodusuSneha) · [LinkedIn](https://linkedin.com/in/sneha-bodusu)
