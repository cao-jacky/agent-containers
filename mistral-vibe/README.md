# Mistral Vibe

Mistral Vibe is an AI coding assistant powered by Mistral's Devstral model. Access it via Free, Pro, or Team plans, or get an API key from Mistral Studio.

## Building

```bash
make mistral-vibe
```

## Configuration

Before running Mistral Vibe, you'll need to configure your API key. You can obtain an API key from [Mistral Studio](https://console.mistral.ai/).

### Setting up API Key

Set your Mistral API key as an environment variable:

```bash
export MISTRAL_API_KEY="your-api-key-here"
```

Or pass it directly when running the container (see below).

## Run Instructions

### Basic Usage

```bash
docker run -it --rm \
  --user $(id -u):$(id -g) \
  -e MISTRAL_API_KEY \
  -v $(pwd):/app:rw \
  mistral-vibe
```

### With Podman

```bash
podman run -it --rm \
  --userns=keep-id \
  -e MISTRAL_API_KEY \
  -v $(pwd):/app:rw \
  mistral-vibe
```

### Persisting Configuration

To persist configuration across sessions:

```bash
docker run -it --rm \
  --user $(id -u):$(id -g) \
  -e MISTRAL_API_KEY \
  -v $(pwd):/app:rw \
  -v ~/.config/mistral-vibe:/home/node/.config/mistral-vibe:rw \
  mistral-vibe
```

### Shell Function (Recommended)

Add this to your `.bashrc` or `.zshrc` for easier launching:

```bash
function mistral-vibe() {
  if type podman >/dev/null; then
    LAUNCHER="podman run --userns=keep-id"
  else
    LAUNCHER="docker run --user $(id -u):$(id -g)"
  fi
  
  $LAUNCHER -it --rm \
    -e MISTRAL_API_KEY \
    -v $(pwd):/app:rw \
    -v ~/.config/mistral-vibe:/home/node/.config/mistral-vibe:rw \
    mistral-vibe "$@"
}
```

Then simply run:

```bash
mistral-vibe
```

## References

* [Mistral Vibe](https://mistral.ai/vibe/)
* [Mistral Studio](https://console.mistral.ai/)
* [Mistral Docs](https://docs.mistral.ai/)
* [Installation Guide](https://mistral.ai/vibe/install.sh)
