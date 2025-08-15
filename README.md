# Ollama and OpenWebUI Docker Deployment

This project provides a Docker Compose configuration to deploy [Ollama](https://ollama.ai/) and [OpenWebUI](https://github.com/open-webui/open-webui) on a local host using Docker. The setup includes GPU support for Ollama and connects both services via a shared Docker network.

## Prerequisites

- [Docker](https://www.docker.com/) and [Docker Compose](https://docs.docker.com/compose/) installed on your system.
- NVIDIA GPU and [NVIDIA Container Toolkit](https://github.com/NVIDIA/nvidia-docker) for GPU support (if applicable).
- Basic knowledge of Docker and containerized applications.

## Project Structure

- `docker-compose.yml`: Defines the services for Ollama and OpenWebUI, including network and volume configurations.
- `./ollama/ollama`: Persistent storage for Ollama models and configurations.
- `./ollama/open-webui`: Persistent storage for OpenWebUI data.

## Setup Instructions

1. **Clone the Repository**  
   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. **Create Required Directories**  
   Ensure the directories for volumes exist:
   ```bash
   mkdir -p ./ollama/ollama ./ollama/open-webui
   ```

3. **Deploy with Docker Compose**  
   Run the following command to start the services:
   ```bash
   docker-compose up -d
   ```

4. **Access the Services**  
   - **Ollama API**: Available at `http://localhost:11434`
   - **OpenWebUI**: Available at `http://localhost:8080`

## Configuration Details

### Ollama Service
- **Image**: `ollama/ollama:latest`
- **Ports**: Maps `11434` (Ollama API) to the host.
- **Volumes**:
  - `./ollama/ollama:/root/.ollama`: Persists Ollama models and configurations.
  - `./code:/code`: Mounts the current directory for additional data or scripts.
- **GPU Support**: Configured to use one NVIDIA GPU.
- **Restart Policy**: Always restarts unless manually stopped.
- **Container Name**: `ollama`

### OpenWebUI Service
- **Image**: `ghcr.io/open-webui/open-webui:main`
- **Ports**: Maps `8080` to the host for web access.
- **Volumes**: `./ollama/open-webui:/app/backend/data` for persistent storage.
- **Environment**: Configures OpenWebUI to communicate with the Ollama API at `http://ollama:11434/api`.
- **Depends On**: Ensures the `ollama` service is running before starting.
- **Restart Policy**: Restarts unless explicitly stopped.
- **Container Name**: `open-webui`

### Network
- Both services are connected via a custom Docker network (`ollama-docker`) for seamless communication.

## Usage

- **Ollama**: Use the API at `http://localhost:11434` to interact with AI models.
- **OpenWebUI**: Access the web interface at `http://localhost:8080` to interact with Ollama models through a user-friendly UI.

## Stopping the Services

To stop and remove the containers:
```bash
docker-compose down
```

## Notes

- Ensure the NVIDIA Container Toolkit is properly configured if GPU support is needed.
- Adjust the `docker-compose.yml` file for additional environment variables or configurations as needed.
- For production use, consider securing the API endpoints and adding authentication to OpenWebUI.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.