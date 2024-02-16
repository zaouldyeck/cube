# cube

WIP!


# A test run

From a Linux host:

sudo CUBE_WORKER_HOST=localhost CUBE_WORKER_PORT=5555 CUBE_MANAGER_HOST=localhost CUBE_MANAGER_PORT=5556 ./cube

From another terminal you could POST some json:

curl -v -X POST localhost:5556/tasks -d @task1.json

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
