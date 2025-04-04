# RunPod SSH Setup

A simple CLI tool to manage SSH config entries for [RunPod](https://www.runpod.io/). It
lets you add or update a `Host` block in your `~/.ssh/config` file automatically.

## Example

```bash
runpod_ssh_setup \
  --host runpod \
  --ssh_cmd "ssh root@157.517.221.29 -p 19090 -i ~/.ssh/id_ed25519"
```

This will either replace an existing `Host runpod` block in your `~/.ssh/config`, or add
one if it does not exist:

```txt
Host runpod
    HostName 157.517.221.29
    User root
    Port 19090
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
```

> **Tip**: You can copy the exact `--ssh_cmd` parameter directly from the RunPod
> Console: **Pods** → **_your pod_** → **Connect** → **Connection Options** → **SSH** →
> **SSH over exposed TCP**.

## Options

- `--config`: Path to your SSH config file (default: `~/.ssh/config`).
- `--host`: The alias to use in the `Host <ALIAS>` entry (required).
- `--disable_host_key_checking`: If present, adds lines that disable host key checks.
- `--ssh_cmd`: Must be in the exact format
  `ssh <USER>@<HOST> -p <PORT> -i <IDENTITY_FILE>`, as provided by RunPod.

### Disabling Host Key Checking

If you add `--disable_host_key_checking`, it adds the following lines to disable host
key checks to the `Host` block:

```
    UserKnownHostsFile /dev/null
    StrictHostKeyChecking no
```

By default, host key checking is enabled.

> **Security Note**: Disabling host key checking can be convenient (for convenience when
> dealing with frequently changing hosts or cloud instances), but it exposes you to
> potential man-in-the-middle attacks.
>
> **We recommend not to disable host key checks for
> production or untrusted environments.**

## Installation

If you have [Poetry](https://python-poetry.org/) installed:

```bash
poetry lock
poetry install
```

Then run via:

```bash
poetry run runpod_ssh_setup ...
```

Or build and install:

```bash
poetry build
pipx install dist/runpod_ssh_setup-*.whl
```

Then use `runpod_ssh_setup` directly.

## License

[MIT License](LICENSE)
