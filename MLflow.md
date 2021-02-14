## MLFlow

Running mlflow

```
❯ mlflow ui -p 5001 -h 0.0.0.0
```

## Preparation
```
❯ ls -l
total 12K
-rw-rw-r--  1 ng ng   83 Feb 14 09:06 Dockerfile
❯ docker build -t mlflow:latest .
❯ docker images
REPOSITORY               TAG       IMAGE ID       CREATED         SIZE
rocm3.7_pytorch1.7       latest    77449f311ac6   2 hours ago     11.5GB
mlflow                   latest    d71c461980f9   3 hours ago     683MB
continuumio/miniconda3   latest    52daacd3dd5d   2 months ago    437MB

```

## Setup

- Start mlflow docker container
    ```
    ❯ docker run -it --rm --name mlflow -p 5000:5000 -v /home/ng/flow/mlruns:/mlruns mlflow:latest
    (base) root@cd39867c5db9:/app# cd /mlruns/
    (base) root@cd39867c5db9:/mlruns# mlflow server --backend-store-uri . --host 0.0.0.0
    ```

- Start `mlflow ui` on your host PC/Laptop terminal
    ```
    ❯ mlflow ui --host 0.0.0.0 --port 5001
    ```

- Running ROCm PyTorch Container

    ```
    ❯ docker run -it -v $HOME:/home --privileged --rm --device=/dev/kfd --device=/dev/dri --group-add video rocm3.7_pytorch1.7:latest
    ❯ docker exec -it <container-id> /bin/bash
    root@910660e8a36d:/home/quickstart#python mlflow_tracking.py
    root@910660e8a36d:/home/flow#python toy_model.py
    ```

- Remove unwanted runs
    ```
    ❯ mlflow gc
    ```

- Save container state
    ```
    ❯ docker commit -m 'save status' <container-id> mlflow:latest
    ```
    