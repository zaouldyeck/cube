# cube

# What is this?

A simple Docker container orchestrator I created as a learning Go project.
It sets up a manager component and worker processes, allowing you to schedule
tasks (containers) onto the workers using the manager API.


# How to build

```
GOOS=linux GOARCH=amd64 go build .
```

# CLI flags

```
./cube worker --help
./cube manager --help
```

# How do I use this with persistent state maintained (for a test run locally)?

From a Linux host:

```
# Setup workers from one terminal per worker
sudo ./cube worker --port 5556 -d persistent
sudo ./cube worker --port 5557 -d persistent
sudo ./cube worker --port 5558 -d persistent
```

```
# Setup the manager from another terminal
sudo ./cube manager -w 'localhost:5556,localhost:5557,localhost:5558' -d persistent
```

From another terminal you could POST some json. Here is an example of submitting a task to the manager:

```
sudo ./cube run --filename task1.json
```

How to stop a task:

```
sudo ./cube stop bb1d59ef-9fc1-4e4b-a44d-db571eeed203
```

Example JSON:

```
{
    "ID": "a7aa1d44-08f6-443e-9378-f5884311019e",
    "State": 2,
    "Task": {
        "State": 1,
        "ID": "bb1d59ef-9fc1-4e4b-a44d-db571eeed203",
        "Name": "test-task",
        "Image": "<some_image>",
        "ExposedPorts": {
            "7777/tcp": {}
        },
        "PortBindings": {
            "7777/tcp": "7777/tcp"
        },
        "HealthCheck": "/health"
    }
}
```
