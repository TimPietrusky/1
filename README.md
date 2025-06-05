# ComfyUI Serverless Deployment

This repository contains a Dockerfile for deploying a ComfyUI workflow with specific models and custom nodes in a serverless environment like RunPod.

## Included Components

- **Base Image**: `runpod/worker-comfyui:5.1.0-base` - A clean base image with ComfyUI, comfy-cli, and ComfyUI-Manager
- **Custom Nodes**: 
  - [ComfyUI-KJNodes](https://github.com/kijai/ComfyUI-KJNodes)
- **Models**:
  - [FLUX1-Schnell](https://huggingface.co/Comfy-Org/flux1-schnell) - A fast and efficient image generation model
  - [RealESRGAN_x2](https://huggingface.co/ai-forever/Real-ESRGAN) - An upscaling model for enhancing image resolution

## Workflow

This Docker image is designed to run the `flux-schnell-RealESRGAN_x2 (2).json` workflow in ComfyUI. The workflow utilizes the FLUX1-Schnell model for image generation and RealESRGAN_x2 for upscaling.

## Usage

### Building the Docker Image

```bash
docker build -t comfyui-flux-schnell:latest .
```

### Running Locally

```bash
docker run -p 8188:8188 comfyui-flux-schnell:latest
```

Then access ComfyUI at http://localhost:8188

### Deploying on RunPod

1. Push your Docker image to a container registry (Docker Hub, GitHub Container Registry, etc.)
2. Create a new RunPod template using your Docker image
3. Deploy a serverless endpoint using your template
4. Use the RunPod API to send requests to your endpoint

## RunPod Serverless API Usage

Example of using the RunPod API to run your workflow:

```python
import requests
import json
import base64

# Your RunPod API endpoint and API key
RUNPOD_ENDPOINT = "https://api.runpod.ai/v2/YOUR_ENDPOINT_ID/run"
API_KEY = "YOUR_API_KEY"

# Load your workflow JSON
with open("path/to/workflow.json", "r") as f:
    workflow = json.load(f)

# Prepare the request payload
payload = {
    "input": {
        "workflow": workflow,
        # Add any input parameters your workflow needs
    }
}

# Make the API request
headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type": "application/json"
}

response = requests.post(RUNPOD_ENDPOINT, headers=headers, json=payload)
print(response.json())
```

## Development

To modify this Docker image for your own workflows:

1. Update the Dockerfile to include any additional custom nodes or models
2. Build and test the Docker image locally
3. Push your changes to this repository
4. Deploy your updated image to RunPod

## License

This project is provided as-is. The included models and custom nodes have their own respective licenses.