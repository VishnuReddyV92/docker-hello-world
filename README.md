Docker Hello World Project
A simple demonstration of Docker containerization with Node.js and Python applications, managed using Docker Compose.

🚀 Project Overview
This project contains two simple "Hello World" applications:

Node.js App: HTTP server running on port 3000

Python Flask App: Web application running on port 5000

Both applications are containerized using Docker and orchestrated with Docker Compose.

📁 Project Structure
text
docker-hello-world/
├── docker-compose.yml
├── README.md
├── node-app/
│   ├── Dockerfile
│   ├── app.js
│   └── package.json
└── python-app/
    ├── Dockerfile
    ├── app.py
    └── requirements.txt
🛠️ Prerequisites
Docker Desktop installed

Git installed

Basic terminal knowledge

📝 Step-by-Step Setup
Step 1: Create Project Structure
bash
mkdir docker-hello-world
cd docker-hello-world
Step 2: Create Node.js Application
bash
mkdir node-app
cd node-app
Create node-app/app.js:

javascript
const http = require('http');

const hostname = '0.0.0.0';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World from Node.js Docker Container!\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
Create node-app/package.json:

json
{
  "name": "node-docker-app",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  }
}
Create node-app/Dockerfile:

dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]
Step 3: Create Python Application
bash
cd ..
mkdir python-app
cd python-app
Create python-app/app.py:

python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World from Python Flask Docker Container!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
Create python-app/requirements.txt:

txt
Flask==2.3.3
Create python-app/Dockerfile:

dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
Step 4: Create Docker Compose File
bash
cd ..
Create docker-compose.yml:

yaml
services:
  node-app:
    build: ./node-app
    ports:
      - "3000:3000"
    container_name: hello-node-app

  python-app:
    build: ./python-app
    ports:
      - "5000:5000"
    container_name: hello-python-app
Step 5: Build and Run the Applications
bash
# Build and start all services
docker compose up -d --build

# Check status
docker compose ps
Step 6: Test the Applications
bash
# Test Node.js app
curl http://localhost:3000

# Test Python app
curl http://localhost:5000

# Or open in browser:
# http://localhost:3000
# http://localhost:5000
🎯 Expected Results
After running all steps, you should see:

Node.js App: "Hello World from Node.js Docker Container!" at http://localhost:3000

Python App: "Hello World from Python Flask Docker Container!" at http://localhost:5000

📊 Useful Commands
Management Commands
bash
# Start all services
docker compose up -d

# Stop all services
docker compose down

# View logs
docker compose logs

# View specific service logs
docker compose logs node-app

# Follow logs in real-time
docker compose logs -f

# Check service status
docker compose ps

# Rebuild specific service
docker compose build node-app
Docker Commands Practice
bash
# List running containers
docker ps

# List all containers
docker ps -a

# List Docker images
docker images

# View container logs
docker logs hello-node-app

# Execute command in container
docker exec hello-node-app node --version

# Remove all unused containers/networks
docker system prune
🐛 Troubleshooting
Common Issues
Port already in use:

bash
# Stop any services using ports 3000/5000
sudo lsof -i :3000
sudo lsof -i :5000
Container not starting:

bash
# Check logs for errors
docker compose logs python-app
Build failures:

bash
# Rebuild specific service
docker compose build --no-cache python-app
Permission issues:

bash
# Reset Docker if needed
docker system prune -a
🧹 Cleanup
bash
# Stop and remove all containers
docker compose down

# Remove all images
docker rmi docker-hello-world-node-app docker-hello-world-python-app

# Remove unused Docker resources
docker system prune
📚 Learning Outcomes
✅ Dockerfile Creation: FROM, WORKDIR, COPY, RUN, EXPOSE, CMD
✅ Image Building: docker build -t name .
✅ Container Management: docker run, docker ps, docker logs
✅ Docker Compose: Multi-service orchestration
✅ Multi-language Support: Node.js and Python containers
✅ Networking: Port mapping and container communication

