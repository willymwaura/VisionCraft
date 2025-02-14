# VisionCraft API Documentation

## Official links:
- [Telegram channel](https://t.me/visioncraft_channel)
- [Website](https://visioncraft.vercel.app)
- [Android app](https://github.com/VisionCraft-org/vision-craft-mobile)
- [Flutter package](https://pub.dev/packages/flutter_vision_craft)

## Table of Contents
- [Introduction](#introduction)
- [Functional Testing](#functional-testing)
- [Obtaining an API Key](#obtaining-an-api-key)
- [Interacting with the API](#interacting-with-the-api)
  - [Image Generation with Stable Diffusion 1.x](#image-generation)
    - [Available Models](#available-models)
    - [Available Samplers](#available-samplers)
    - [Available Loras](#available-loras)
    - [Image generation](#image-generation)
  - [Image Generation with Stable Diffusion XL](#image-generation-xl)
    - [Available XL Models](#available-xl-models)
    - [Image generation](#stable-diffusion-xl)
  - [Image to Image generation](#image-to-image)
  - [LLM generation](#llm-generation)
    - [Available LLM models](#available-llm-models)
    - [Text generation](#text-generation)
  - [Text to GIF generation](#text-to-gif)
  - [Upscale Image](#upscale-image)
- [Key Limitations](#key-limitations)
- [Contact Information](#contact-information)

## Introduction

The VisionCraft API documentation is designed for users who wish to interact with the API for image generation. This documentation will provide you with a comprehensive overview of the functionality and usage guidelines of the API.

## Functional testing

If you want to try how well my API generates images, then go to the [official site](https://visioncraft.vercel.app). There you can try all the models available in the API.

## Obtaining an API Key

To start using the VisionCraft API, you need to obtain an API key. This key is essential for every request to the API. You can obtain the key by following these steps:

1. **Subscribe to the [VisionCraft Telegram channel](https://t.me/visioncraft_channel).**

   To receive an API key, users are **required** to be subscribed to this channel. If you are already subscribed, proceed to the next step.

2. **Send the `/key` command to the [VisionCraft bot](https://t.me/VisionCraft_bot).**

   After subscribing to the channel, send this command to the bot in the chat. The bot's response will contain your API key. Please save this key, as it will be used for authentication when interacting with the API. Key generation takes approximately 15 seconds.

3. **Secure your key.**

   It is essential to protect your API key. Do not share it with other users, and do not post it in public places. Otherwise, your key will be banned, and generating a new key is not possible. One Telegram account is associated with one key.

# Interacting with the API

## Image Generation

### Available Models

You can retrieve a list of available models for image generation. Each model has its unique characteristics and generation style.

#### Request:
```
GET https://visioncraft-rs24.koyeb.app/models
```

#### Response:
```
["3guofeng3_v3.4", "absolutereality_v1.6", "absolutereality_v1.8.1", "amIReal_v4.1", "analog_diffusion_v1", "anything_v3.0", "anything_v4.5", "anything_V5", ...]
```

### Available Samplers

You can retrieve a list of available samplers for image generation.

If you do not know which sampler to choose, I recommend reading [this article](https://stable-diffusion-art.com/samplers/).

#### Request:
```
GET https://visioncraft-rs24.koyeb.app/samplers
```

#### Response:
```
["Euler", "Euler a", "LMS", "Heun", "DPM2", "DPM2 a", ...]
```

### Available Loras

You can retrieve a list of available Loras for image generation.

#### Request:
```
GET https://visioncraft-rs24.koyeb.app/loras
```

#### Response:
```
["0mib3_v10", "3DMM_V12", "age_slider_v20", "arcane_offset", "AstralMecha", "epi_noiseoffset2", "eye_size_slider_v1", ...]
```


After selecting a specific model, you can generate images using the API. To do this, you need to make a POST request and provide the necessary parameters.

### Stable Diffusion

#### Request:
```
POST https://visioncraft-rs24.koyeb.app/generate
```

#### Request Parameters:
- `model` (string) - the name of the chosen model
- `sampler` (string) - the name of the chosen sampler
- `prompt` (string) - a text prompt for generation
- `negative_prompt` (string) (optional) - text prompt that the model should not be drawn on the picture.
- `image_count` (integer) - the number of images to generate (up to 5 in a single request)
- `token` (string) - your API key
- `cfg_scale` (integer) (optional: default is 10) - the CFG Scale (0-20)
- `steps` (integer) (optional: default is 30)- the number of steps (1-50)
- `loras` (dict) (optional) - a dictionary in which the key is the name Lora, and the meaning is its weight.

**About parameters**

**CFG_Scale**: How strongly the image should conform to the text - lower values produce more creative results.

**Steps**: How many times to improve the generated image iteratively; higher values take longer; very low values can produce bad results.

**Loras**: LoRA models are small trained models for Stable Diffusion that make additional changes to image generation and are used in conjunction with standard models.

#### Request Example:
```
{
  "model": "anything_V5",
  "sampler": "Euler",
  "prompt": "Beautiful landscape",
  "negative_prompt": "canvas frame, cartoon, 3d, ((disfigured)), ((bad art)), ((deformed)), ((extra limbs)), ((close up)), ((b&w)), weird colors, blurry, (((duplicate))), ((morbid)), ((mutilated)), [out of frame], extra fingers, mutated hands, ((poorly drawn hands)), ((poorly drawn face)), (((mutation))), ((ugly)), (((bad proportions))), (malformed limbs), ((missing arms)), ((missing legs)), (((extra arms))), (((extra legs))), (fused fingers), (too many fingers), (((long neck))), Photoshop, video game, tiling, poorly drawn feet, body out of frame",
  "image_count": 3,
  "token": "your_api_key",
  "cfg_scale": 8,
  "steps": 30,
  "loras": {"3DMM_V12": 1, "GrayClay_V1.5.5": 2}
}
```

The response to this request will contain a list of links to the generated images.

**Python Example:**

```
# Python code for interacting with VisionCraft API
import requests

# Define the API endpoint
api_url = "https://visioncraft-rs24.koyeb.app"

# Obtain your API key
api_key = "your_api_key"

# Generate images using a specific model
model = "anything_V5"
sampler = "Euler"
image_count = 3
cfg_scale = 8
steps = 30
loras = {"3DMM_V12": 1, "GrayClay_V1.5.5": 2}

# Set up the data to send in the request
data = {
    "model": model,
    "sampler": sampler,
    "prompt": "Beautiful landscape",
    "negative_prompt": "canvas frame, cartoon, 3d, ((disfigured)), ((bad art)), ((deformed)),((extra limbs)),((close up)),((b&w)), weird colors, blurry, (((duplicate))), ((morbid)), ((mutilated)), [out of frame], extra fingers, mutated hands, ((poorly drawn hands)), ((poorly drawn face)), (((mutation))), (((deformed))), ((ugly)), blurry, ((bad anatomy)), (((bad proportions))), ((extra limbs)), cloned face, (((disfigured))), out of frame, ugly, extra limbs, (bad anatomy), gross proportions, (malformed limbs), ((missing arms)), ((missing legs)), (((extra arms))), (((extra legs))), mutated hands, (fused fingers), (too many fingers), (((long neck))), Photoshop, video game, ugly, tiling, poorly drawn hands, poorly drawn feet, poorly drawn face, out of frame, mutation, mutated, extra limbs, extra legs, extra arms, disfigured, deformed, cross-eye, body out of frame, blurry, bad art, bad anatomy, 3d render",
    "image_count": image_count,
    "token": api_key,
    "cfg_scale": cfg_scale,
    "steps": steps,
    "loras": loras
}

# Send the request to generate images
response = requests.post(f"{api_url}/generate", json=data, verify=True)

# Extract the image URLs from the response
image_urls = response.json()["images"]

# Download and save the generated images
for i, image_url in enumerate(image_urls):
    # Get the image data from the URL
    response = requests.get(image_url)
    # Save the image locally
    with open(f"generated_image_{i}.png", "wb") as f:
        f.write(response.content)
```

## Image generation XL

### Available XL Models

You can retrieve a list of available models for image generation XL. Each model has its unique characteristics and generation style.

#### Request:
```
GET https://visioncraft-rs24.koyeb.app/models-xl
```

#### Response:
```
["sdxl-base","sdxl-turbo", "deliberate-v3","dreamshaper-v8", "swizz8", ...]
```

After selecting a specific model, you can generate images using the API. To do this, you need to make a POST request and provide the necessary parameters.

### Stable Diffusion XL

#### Request:
```
POST https://visioncraft-rs24.koyeb.app/generate-xl
```

#### Request Parameters:
- `model` (string) - the name of the chosen XL model
- `prompt` (string) - a text prompt for generation
- `negative_prompt` (string) (optional) - text prompt that the model should not be drawn on the picture.
- `image_count` (integer) - the number of images to generate (up to 2 in a single request)
- `token` (string) - your API key
- `height` (integer) - generated image height (minimum 64, maximum 1024), default is 1024
- `width` (integer) - generated image width (minimum 64, maximum 1024), default is 1024
- `enhance` (bool) - whether to improve the quality of the generated image, default is `False`.

#### Request Example:
```
{
  "model": "sdxl-base",
  "prompt": "Beautiful landscape",
  "negative_prompt": "bad quality",
  "image_count": 1,
  "token": "your_api_key",
  "height: 768,
  "width": 1024,
  "enhance": False
}
```

The response to this request will contain a list of links to the generated images.

**Python Example:**

```
# Python code for interacting with VisionCraft API
import requests

# Define the API endpoint
api_url = "https://visioncraft-rs24.koyeb.app"

# Obtain your API key
api_key = "your_api_key"

# Set up the data to send in the request
data = {
    "model": "sdxl-base",
    "prompt": "Beautiful landscape",
    "negative_prompt": "bad quality",
    "image_count": 1,
    "token": api_key,
    "width": 1024,
    "height": 768,
    "enhance": False
}

# Send the request to generate images
response = requests.post(f"{api_url}/generate-xl", json=data, verify=True)

# Extract the image URLs from the response
image_urls = response.json()["images"]

# Download and save the generated images
for i, image_url in enumerate(image_urls):
    # Get the image data from the URL
    response = requests.get(image_url)
    # Save the image locally
    with open(f"generated_image_{i}.png", "wb") as f:
        f.write(response.content)
```



## Image to Image

#### Request:
```
POST https://visioncraft-rs24.koyeb.app/img2img
```

#### Request Parameters:
- `prompt` (string) - a text prompt for generation
- `token` (string) - your API key
- `steps` (integer) (optional: default is 7) - the number of steps (1-10)
- `image` (base64 string) - your image in base64 format
- `strength` (float) (optional: default is 0.7) - how similar the generated image will be to the original. The lower the number, the more similar the generated image will be to the original. (0.0-1.0)

#### Request Example:
```
{
  "prompt": "Beautiful lady",
  "token": "your_api_key",
  "image": "your_image_in_base64_format",
  "steps": 7,
  "strength": 0.7
}
```

The response to this request will contain a list of links to the generated images.

**Python Example:**

```
# Python code for interacting with VisionCraft API
import requests, base64

# Define the API endpoint
api_url = "https://visioncraft-rs24.koyeb.app"

# Obtain your API key
api_key = "your_api_key"

with open("my_image.png", "rb") as image:
  image_base64 = base64.b64encode(image.read()).decode("utf-8")

# Set up the data to send in the request
data = {
    "prompt": "Beautiful girl",
    "token": api_key,
    "image": image_base64,
    "steps": 7,
    "strength": 0.7
}

# Send the request to generate images
response = requests.post(f"{api_url}/img2img", json=data, verify=True)

# Extract the image URLs from the response
image_url = response.json()["images"][0]

# Get the image data from the URL
response = requests.get(image_url)
# Save the image locally
with open(f"generated_image.png", "wb") as f:
    f.write(response.content)
```

## LLM Generation

### Available LLM Models

You can retrieve a list of available models for text generation.

#### Request:
```
GET https://visioncraft-rs24.koyeb.app/models-llm
```

#### Response:
```
["nous-capybara-7b", "mistral-7b-instruct", "zephyr-7b-beta", "openchat-7b", ...]
```

### Text generation
After selecting model, you can generate text.

#### Request:
```
POST https://visioncraft-rs24.koyeb.app/llm
```

#### Request Parameters:
- `model` (string) - the name of the chosen LLM model
- `token` (string) - your API key
- `messages` (list) - messages to the LLM

#### Request Example:
```
{
  "model": "mistral-7b-instruct",
  "token": "your_api_key",
  "messages": [
    {"role": "user", "content": "What is the meaning of life?"}
  ]
}
```

The response to this request will contain a response from LLM model.

**Python Example:**

```
# Python code for interacting with VisionCraft API
import requests, json

response = requests.post(
  url="https://visioncraft-rs24.koyeb.app/llm",
  data=json.dumps({
    "token": "your_token",
    "model": "mistral-7b-instruct",
    "messages": [
      {"role": "system", "content": "You are a helpful assistant."},
      {"role": "user", "content": "Tell me a joke."},
      {"role": "assistant", "content": "Sure, here's a joke: Why don't scientists trust atoms? Because they make up everything!"},
      {"role": "user", "content": "That's a good one! Tell me another."},
      {"role": "assistant", "content": "Why did the computer go to therapy? It had too many bytes of emotional baggage."},
      {"role": "user", "content": "Haha, nice! What's the weather like today?"}
    ]
  })
)

print(response.json())
```


### Text to GIF

#### Request:
```
POST https://visioncraft-rs24.koyeb.app/generate-gif
```

#### Request Parameters:
- `token` (string) - your API key
- `sampler` (string) - the name of the chosen sampler
- `prompt` (string) - a text prompt for generation
- `negative_prompt` (string) (optional) - text prompt that the model should not be drawn on the picture.
- `cfg_scale` (integer) (optional: default is 10) - the CFG Scale (0-20)
- `steps` (integer) (optional: default is 30)- the number of steps (1-50)

#### Request Example:
```
{
  "sampler": "Euler",
  "prompt": "Beautiful landscape",
  "negative_prompt": "canvas frame, cartoon, 3d, ((disfigured)), ((bad art)), ((deformed)), ((extra limbs)), ((close up)), ((b&w)), weird colors, blurry, (((duplicate))), ((morbid)), ((mutilated)), [out of frame], extra fingers, mutated hands, ((poorly drawn hands)), ((poorly drawn face)), (((mutation))), ((ugly)), (((bad proportions))), (malformed limbs), ((missing arms)), ((missing legs)), (((extra arms))), (((extra legs))), (fused fingers), (too many fingers), (((long neck))), Photoshop, video game, tiling, poorly drawn feet, body out of frame",
  "token": "your_api_key",
  "cfg_scale": 8,
  "steps": 30
}
```

The response to this request will contain a list of link to the generated GIF.

**Python Example:**

```
# Python code for interacting with VisionCraft API
import requests

# Define the API endpoint
api_url = "https://visioncraft-rs24.koyeb.app"

# Obtain your API key
api_key = "your_api_key"

# Generate images using a specific model
sampler = "Euler"
cfg_scale = 8
steps = 30

# Set up the data to send in the request
data = {
    "sampler": sampler,
    "prompt": "Beautiful landscape",
    "negative_prompt": "canvas frame, cartoon, 3d, ((disfigured)), ((bad art)), ((deformed)),((extra limbs)),((close up)),((b&w)), weird colors, blurry, (((duplicate))), ((morbid)), ((mutilated)), [out of frame], extra fingers, mutated hands, ((poorly drawn hands)), ((poorly drawn face)), (((mutation))), (((deformed))), ((ugly)), blurry, ((bad anatomy)), (((bad proportions))), ((extra limbs)), cloned face, (((disfigured))), out of frame, ugly, extra limbs, (bad anatomy), gross proportions, (malformed limbs), ((missing arms)), ((missing legs)), (((extra arms))), (((extra legs))), mutated hands, (fused fingers), (too many fingers), (((long neck))), Photoshop, video game, ugly, tiling, poorly drawn hands, poorly drawn feet, poorly drawn face, out of frame, mutation, mutated, extra limbs, extra legs, extra arms, disfigured, deformed, cross-eye, body out of frame, blurry, bad art, bad anatomy, 3d render",
    "token": api_key,
    "cfg_scale": cfg_scale,
    "steps": steps
}

# Send the request to generate images
response = requests.post(f"{api_url}/generate-gif", json=data, verify=True)

# Extract the image URLs from the response
image_urls = response.json()["images"]

# Download and save the generated images
for i, image_url in enumerate(image_urls):
    # Get the image data from the URL
    response = requests.get(image_url)
    # Save the image locally
    with open(f"generated_image_{i}.gif", "wb") as f:
        f.write(response.content)
```

### Upscale Image

#### Request:
```
POST https://visioncraft-rs24.koyeb.app/upscale
```

#### Request Parameters:
- `token` (string) - your API key
- `image` (bytes-like object) - your image for upscaling

#### Request Example:
```
{
  "token": "your_api_key",
  "image": "your image in bytes"
}
```

The response to this request will contain a list of links to the generated images.

**Python Example:**

```
import requests, base64

def upscale_request(image):
    b = base64.b64encode(image).decode('utf-8')
    payload = {
        "token": "your_token",
        "image": b
    }
    url = 'https://visioncraft-rs24.koyeb.app/upscale'
    headers = {"content-type": "application/json"}

    resp = requests.post(url, json=payload, headers=headers)
    return resp.content

image = open('my_image.png', 'rb').read()
upscaled_image = upscale_request(image)
with open('upscaled_image.png', 'wb') as f:
    f.write(upscaled_image)
```


## Key Limitations

It's important to note that your API key is linked to your subscription to the VisionCraft Telegram channel. If you unsubscribe from the channel, your key will cease to function. However, when you resubscribe to the channel, the key automatically renews its functionality.

## Contact Information

If you have any questions or requests, feel free to reach out to us. We are always ready to assist you. For communication, use my [Telegram](https://t.me/metimol).
