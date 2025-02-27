# PyTorch VSCode Server with CUDA

Docker images for machine learning development environments using CUDA and PyTorch and for remote development via VSCode and SSH server

## Features

- CUDA 12.1
- Python 3.11.9 (Conda)
- PyTorch 2.2.2
- Code Server
- SSH Server
- \+ Other tools (e.g. git, wget, curl, unzip, etc.)
- \+ Python packages (e.g. numpy, pandas, matplotlib, tensorboard, etc.)
- ffmpeg + libx264 + others (by edvard, Dockerfile_edz)

## Usage

UPDATE: 2024-10-11
ffmpeg + libx264 + others (by edvard, Dockerfile_edz)


```bash
# modify based on you device
docker run -d \
  -p 8078:443 \
  -p 8079:22 \
  -p 8088:8088 \
  -p 8089:8089 \
  --runtime nvidia \
  -e PASSWORD="hello_world" \
  --shm-size 64g \
  -v /data/zengzihua:/workspace \
  -v /mnt/cephfs2:/mnt/cephfs2 \
  --name pytorch-vscode-server \
  harbor.bigo.sg/bigo_ai/zengzihua/code_server:torch2-cuda121-patch
```

- Access VSCode Server: `https://localhost:5443`
- SSH: `ssh ubuntu@localhost -p 5022 -i ~/.ssh/id_rsa` (only key-based authentication, If you do not set up `SSH_PUBLIC_KEY`, SSH Server will not run.)

If you want to use SSH Server, you need to set `SSH_PUBLIC_KEY` environment variable.

```bash
docker run -d \
  -p 5443:443 \
  -p 5022:22 \
  --gpus '"device=0"' \
  -e PASSWORD="your_vscode_password" \
  -e SSH_PUBLIC_KEY="$(cat ~/.ssh/id_rsa.pub)" \
  --name pytorch-vscode-server \
  ghcr.io/soju06/pytorch-vscode-server:1.0.2-pytorch2.2.2-cuda12.1
```

### Build Arguments

- `UBUNTU_APT_MIRROR`: Set the Ubuntu apt mirror. Default is `""`
- `PYTHON_VERSION`: Set the Python version. Default is `3.11.9`
- `PYTORCH_VERSION`: Set the PyTorch version. Default is `2.2.2`
- `CUDA_VERSION`: Set the CUDA version. Default is `12.1`
- `CONDA_ENVIRONMENT_NAME`: Set the conda environment name. Default is `pytorch`
- `USER`: Set the user name. Default is `ubuntu`
- `GROUP`: Set the group name. Default is `ubuntu`
- `UID`: Set the user id. Default is `1000`
- `GID`: Set the group id. Default is `1000`
- `RESTORE_MIRROR_AFTER_BUILD`: Restore the original apt mirror after build. Default is `true`

### Environment Variables

- `PASSWORD`: Set the password for VSCode Server. Default is `password`
- `SSH_PUBLIC_KEY`: Set the public key for SSH Server. Default is empty
- `HOME`: Set the home directory. Default is `/home/ubuntu`
- `WORKSPACE`: Set the workspace directory. Default is `/workspace`
- `VSCODE_HOME`: Set the VSCode Server home directory. Default is `/workspace/.code-server`
