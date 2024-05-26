# Cube Project Architecture Documentation

## Overview
Cube is a simple Docker container orchestrator written in Go. It provides a lightweight system for managing and scheduling tasks across multiple worker nodes using an API. The project is designed with simplicity and scalability in mind, leveraging BoltDB for state persistence.

## Components
### Manager
- **Responsibilities**: 
  - Manages task distribution to workers.
  - Coordinates worker processes.
  - Exposes API endpoints for task submission and monitoring.
- **Implementation**: 
  - Listens on a designated port for API requests.
  - Maintains a registry of available workers and their statuses.

### Workers
- **Responsibilities**: 
  - Execute tasks as assigned by the manager.
  - Report status back to the manager.
- **Implementation**: 
  - Run as separate processes, each listening on a unique port.
  - Can execute various types of tasks defined by the manager.

### Scheduler
- **Responsibilities**: 
  - Handles the scheduling logic for task distribution.
  - Ensures tasks are balanced across available workers.
- **Implementation**: 
  - Uses algorithms to determine the optimal worker for each task.
  - Can be extended to support different scheduling policies.

### Task State
- **Responsibilities**: 
  - Maintain the state of tasks, including their statuses and results.
- **Implementation**: 
  - Uses BoltDB for persistent storage.
  - Ensures task states are recoverable in case of system restarts.

## API
### Endpoints
- **/tasks**: Submit a new task.
  - **Method**: POST
  - **Payload**: JSON object describing the task.
- **/tasks/{id}**: Get task status.
  - **Method**: GET
  - **Response**: JSON object with task status and result.

## Setup
### Manager Setup
1. Clone the repository.
2. Build the manager component using `go build -o manager ./cmd/manager`.
3. Start the manager: `./manager`.

### Worker Setup
1. Clone the repository on each worker node.
2. Build the worker component using `go build -o worker ./cmd/worker`.
3. Start each worker on a different port: `./worker -port=<port>`.

### Configuration
- The manager needs to be configured with the list of worker nodes and their ports.
- This can be done via a configuration file or environment variables.

## Example Usage
1. Start the manager on the master node.
2. Start workers on several nodes.
3. Submit tasks via the API: `curl -X POST -d '{"task": "example"}' http://manager:port/tasks`.
4. Check task status: `curl http://manager:port/tasks/{id}`.

## State Persistence
- BoltDB is used to store the state of tasks.
- This ensures that task information is persistent and can be recovered in case of failures.

## Future Improvements
- Support for more advanced scheduling algorithms.
- Enhanced API security and authentication.
- Improved monitoring and logging capabilities.
- Scaling the manager to handle a larger number of workers and tasks.
