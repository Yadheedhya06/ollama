# Installing Ollama on Linux

> Note: A one line installer for Ollama is available by running:
>
> ```bash
> curl https://ollama.ai/install.sh | sh
> ```

## Download the `ollama` binary

Ollama is distributed as a self-contained binary. Download it to a directory in your PATH:

```bash
sudo curl -L https://ollama.ai/download/ollama-linux-amd64 -o /usr/bin/ollama
sudo chmod +x /usr/bin/ollama
```

## Start Ollama

NOTE : If you are trying to run Ollama after fresh installation then you can skip to step running model (Since Ollama server should already be running at port number 11434 after a fresh installation)

Start Ollama by running `ollama serve`:

```bash
ollama serve
```

Once Ollama is running, run a model in another terminal session:

```bash
ollama run llama2
```

## Stop Ollama

Stop Ollama by running `systemctl stop ollama`:

```bash
systemctl stop ollama
```

## Install CUDA drivers (optional – for Nvidia GPUs)

[Download and install](https://developer.nvidia.com/cuda-downloads) CUDA.

Verify that the drivers are installed by running the following command, which should print details about your GPU:

```bash
nvidia-smi
```

## Adding Ollama as a startup service (optional)

Create a user for Ollama:

```bash
sudo useradd -r -s /bin/false -m -d /usr/share/ollama ollama
```

Create a service file in `/etc/systemd/system/ollama.service`:

```ini
[Unit]
Description=Ollama Service
After=network-online.target

[Service]
ExecStart=/usr/bin/ollama serve
User=ollama
Group=ollama
Restart=always
RestartSec=3
Environment="HOME=/usr/share/ollama"

[Install]
WantedBy=default.target
```

Then start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable ollama
```

### Viewing logs

To view logs of Ollama running as a startup service, run:

```bash
journalctl -u ollama
```
