# Personal Website CI/CD Pipeline

This project demonstrates a complete CI/CD pipeline for a personal website using Docker, GitHub Actions, and Render.com.

## Project Structure

- `index.html` - Main website content
- `styles.css` - Styling for the website
- `Dockerfile` - Instructions to build the Docker image
- `.dockerignore` - Files to exclude from Docker build
- `.github/workflows/ci-cd.yml` - GitHub Actions workflow for CI/CD

## How It Works

1. **Code Changes**: When you push code to GitHub
2. **CI Pipeline**: GitHub Actions builds the Docker image and pushes it to Docker Hub
3. **CD Pipeline**: Render.com pulls the updated image from Docker Hub and redeploys the website

## Setup Instructions

### Prerequisites

- GitHub account ([Omar679](https://github.com/Omar679))
- Docker Hub account ([umarsabiu](https://hub.docker.com/u/umarsabiu))
- Render.com account
- Docker installed and running locally

### Local Development

1. Clone this repository
2. Build and test the Docker image locally:
   ```bash
   docker build -t omar679/personal-website:latest .
   docker run -p 8080:80 omar679/personal-website:latest
   ```
3. Visit `http://localhost:8080` to see your website

### CI/CD Setup

1. **Push to GitHub**:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/Omar679/personal-website-cicd.git
   git push -u origin main
   ```

2. **Configure GitHub Secrets**:
   - Go to your repository Settings > Secrets and variables > Actions
   - Add these secrets:
     - `DOCKER_USERNAME`: your Docker Hub username (umarsabiu)
     - `DOCKER_PASSWORD`: your Docker Hub password or access token
     - `RENDER_SERVICE_ID`: your Render.com service ID (get from Render dashboard)
     - `RENDER_API_KEY`: your Render.com API key

3. **Deploy to Render.com**:
   - Create a new Web Service on Render.com
   - Connect your GitHub repository
   - Set the build command to: `docker build -t omar679/personal-website:$COMMIT .`
   - Set the start command to: `docker run -p $PORT:80 omar679/personal-website:$COMMIT`
   - (Alternatively, use the Docker Hub auto-deploy method described in the workflow)

4. **Enable Automatic Deployments**:
   - In Render.com, enable auto-deploy from Docker Hub when you push new tags
   - Or use the GitHub Actions workflow to trigger Render deploys

## GitHub Actions Workflow

The workflow (`.github/workflows/ci-cd.yml`) performs these steps:

1. **Trigger**: On push to main branch
2. **Build**: 
   - Set up Docker Buildx
   - Log in to Docker Hub
   - Build and push Docker image with tags: `latest` and git SHA
3. **Deploy** (optional): 
   - Trigger Render.com deploy via API
   - Or rely on Render's Docker Hub integration

## Learning Outcomes

By completing this project, you'll understand:
- Creating reusable Docker images
- Writing GitHub Actions workflows
- Integrating with Docker Hub registry
- Setting up automated deployments
- Basic DevOps pipeline concepts

## Troubleshooting

- **Docker build fails**: Ensure Docker daemon is running
- **GitHub Actions fails**: Check secret configurations
- **Render.com deployment fails**: Verify service has access to Docker Hub image
- **Website not updating**: Check that Render.com is pulling the latest image

## Next Steps

After mastering this basic pipeline, consider:
- Adding automated tests (HTML validation, link checking)
- Implementing blue/green deployments
- Adding environment variables for different deployment stages
- Monitoring and logging integration