## Running the Project with Docker

This project provides Dockerfiles and a `docker-compose.yaml` for running all major services in isolated containers. Below are the key details and instructions for running the project using Docker Compose.

### Project-Specific Requirements
- **Node.js Version:**
  - `typescript-scaffold`: Node 22.14.0
  - `javascript-worker` & `typescript-fake-llm-server`: Node 22.13.1
- **Package Managers:**
  - Uses `pnpm` (v10.4.1) for the scaffold frontend (enabled via Corepack)
  - Uses `npm` for the worker and fake LLM server

### Environment Variables
- The Docker Compose file references an optional `.env` file for environment variables. Uncomment the `env_file` lines in the compose file if you need to provide custom environment variables.
- Example environment variables can be found in `./.env.example`.

### Build and Run Instructions
1. Ensure Docker and Docker Compose are installed.
2. From the project root, run:
   ```sh
   docker compose up --build
   ```
   This will build and start all services defined in `docker-compose.yaml`.

### Service Details & Exposed Ports
- **typescript-scaffold**
  - Context: `./scaffold`
  - Port: `3000` (Vite preview server)
- **javascript-worker**
  - Context: `./worker`
  - Port: `3001` (maps to internal 3000)
- **typescript-fake-llm-server**
  - Context: `./testing/fake-llm-server`
  - Port: `3500`
- All services are connected via the `appnet` bridge network for inter-service communication.

### Special Configuration Notes
- No persistent volumes are required for any service.
- All services run as non-root users for improved security.
- If you need to provide environment variables, copy `.env.example` to `.env` and uncomment the `env_file` lines in the compose file.
- The worker service expects its command to be overridden if you need to pass custom `workerData`.

---

*This section was updated to reflect the current Docker setup for the project. Please refer to the above for the most accurate instructions on running the project with Docker.*
