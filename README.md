# Docker Setup Project

A full-stack application with Node.js backend and React frontend, containerized using Docker and Docker Compose.

## Project Structure

```
Docker-Setup-project/
├── Backend-node/          # Node.js Express backend
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│       └── Server.js
├── my-react-app/          # React frontend (Vite)
│   ├── Dockerfile
│   ├── package.json
│   └── src/
├── docker-compose.yml     # Docker Compose configuration
└── README.md
```

## Prerequisites

Before you begin, ensure you have the following installed on your system:

- **Docker** (version 20.10 or higher)
  - Check installation: `docker --version`
  - [Install Docker](https://docs.docker.com/get-docker/)

- **Docker Compose** (version 2.0 or higher)
  - Check installation: `docker-compose --version`
  - Usually comes bundled with Docker Desktop
  - [Install Docker Compose](https://docs.docker.com/compose/install/)

## Step-by-Step Setup Instructions

### Step 1: Clone or Navigate to the Project

If you haven't already, navigate to the project directory:

```bash
cd "/home/sachinmaurya/Desktop/Sachin M/Docker-Setup-project"
```

### Step 2: Verify Docker is Running

Make sure Docker daemon is running:

```bash
docker ps
```

If you get an error, start Docker:
- **Linux**: `sudo systemctl start docker`
- **Windows/Mac**: Start Docker Desktop application

### Step 3: Build and Start the Containers

Build and start both backend and frontend services using Docker Compose:

```bash
docker-compose up --build
```

**What this does:**
- Builds Docker images for both backend and frontend
- Creates and starts containers for both services
- Sets up networking between containers
- Mounts volumes for hot-reloading during development

**Note:** The first time you run this, it may take a few minutes as it downloads base images and installs dependencies.

### Step 4: Access the Application

Once the containers are running, you can access:

- **Frontend (React App)**: http://localhost:5173
- **Backend API**: http://localhost:5000

You should see:
- Backend: "Node API Running 🚀" message when you visit http://localhost:5000
- Frontend: Your React application interface

### Step 5: View Running Containers (Optional)

To see the status of your containers:

```bash
docker-compose ps
```

Or use Docker directly:

```bash
docker ps
```

### Step 6: View Logs (Optional)

To view logs from all services:

```bash
docker-compose logs
```

To view logs from a specific service:

```bash
docker-compose logs backend
docker-compose logs frontend
```

To follow logs in real-time:

```bash
docker-compose logs -f
```

## Common Commands

### Stop the Application

Stop all running containers:

```bash
docker-compose down
```

### Stop and Remove Volumes

Stop containers and remove volumes (this will remove node_modules from containers):

```bash
docker-compose down -v
```

### Restart the Application

Restart all services:

```bash
docker-compose restart
```

### Rebuild After Code Changes

If you make changes to Dockerfiles or need to rebuild:

```bash
docker-compose up --build
```

### Run in Detached Mode (Background)

Run containers in the background:

```bash
docker-compose up -d
```

### Execute Commands in Containers

Run a command in the backend container:

```bash
docker-compose exec backend sh
```

Run a command in the frontend container:

```bash
docker-compose exec frontend sh
```

## Development Workflow

### Hot Reloading

Both services support hot-reloading:
- **Backend**: Uses nodemon to automatically restart on file changes
- **Frontend**: Vite provides instant HMR (Hot Module Replacement)

Changes to your code will automatically reflect in the running containers thanks to volume mounts.

### Making Code Changes

1. Edit files in `Backend-node/src/` or `my-react-app/src/`
2. Changes are automatically reflected (no need to rebuild)
3. Check the logs if something doesn't work: `docker-compose logs -f`

## Troubleshooting

### Port Already in Use

If you get an error about ports 5000 or 5173 being in use:

**Option 1:** Stop the service using the port
```bash
# Find process using port 5000
lsof -i :5000
# Kill the process (replace PID with actual process ID)
kill -9 <PID>
```

**Option 2:** Change ports in `docker-compose.yml`
```yaml
ports:
  - "5001:5000"  # Change host port to 5001
```

### Container Won't Start

1. Check logs: `docker-compose logs`
2. Rebuild containers: `docker-compose up --build --force-recreate`
3. Remove old containers: `docker-compose down` then `docker-compose up --build`

### Permission Issues (Linux)

If you encounter permission issues:

```bash
sudo docker-compose up --build
```

Or add your user to the docker group:
```bash
sudo usermod -aG docker $USER
# Then log out and log back in
```

### Clean Up Everything

Remove all containers, networks, and volumes:

```bash
docker-compose down -v --rmi all
```

## Services Overview

### Backend Service
- **Container Name**: `backend-node`
- **Port**: 5000
- **Technology**: Node.js with Express
- **Hot Reload**: Nodemon
- **API Endpoint**: http://localhost:5000

### Frontend Service
- **Container Name**: `frontend-react`
- **Port**: 5173
- **Technology**: React with Vite
- **Hot Reload**: Vite HMR
- **URL**: http://localhost:5173

## Environment Variables

### Backend
- `PORT`: Server port (default: 5000)
- `NODE_ENV`: Environment mode (development/production)

### Frontend
- `VITE_API_URL`: Backend API URL (default: http://localhost:5000)

You can modify these in `docker-compose.yml` under the `environment` section.

## Production Deployment

For production, you'll want to:

1. Build optimized production images
2. Use production environment variables
3. Remove volume mounts (or use them differently)
4. Use multi-stage builds in Dockerfiles
5. Set up proper reverse proxy (nginx)

## Additional Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Node.js Documentation](https://nodejs.org/docs/)
- [React Documentation](https://react.dev/)
- [Vite Documentation](https://vitejs.dev/)

## Support

If you encounter any issues:
1. Check the logs: `docker-compose logs`
2. Verify Docker is running: `docker ps`
3. Ensure ports are not in use
4. Try rebuilding: `docker-compose up --build --force-recreate`

---

**Happy Coding! 🚀**

