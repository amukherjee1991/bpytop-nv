# bpytop-nv

[bpytop](https://github.com/aristocratos/bpytop) with an [nvtop](https://github.com/Syllo/nvtop)-style GPU box for NVIDIA cards. You get CPU, GPU, memory, disks, network and processes in one terminal view.

![bpytop-nv full view](https://raw.githubusercontent.com/amukherjee1991/bpytop-nv/main/docs/screenshots/full.png)

## Install

```sh
pip install bpytop-nv
bpytop-nv
```

On distributions that block system-wide `pip` (Ubuntu 23.04+, Debian 12+, Fedora 38+), use [pipx](https://pipx.pypa.io/) instead:

```sh
pipx install bpytop-nv
```

To install the latest code from GitHub:

```sh
pip install git+https://github.com/amukherjee1991/bpytop-nv.git
```

To run from a clone without installing:

```sh
git clone https://github.com/amukherjee1991/bpytop-nv.git
cd bpytop-nv
python3 -m bpytop_nv
```

### Requirements

- Linux
- Python 3.7+
- [psutil](https://github.com/giampaolo/psutil), installed automatically by pip
- The NVIDIA proprietary driver for the GPU box. It ships `libnvidia-ml.so.1`. You don't need `pynvml` or `nvidia-smi`.

If no NVIDIA GPU or driver is found, the GPU box stays hidden and everything else works like stock bpytop.

## The GPU box

![GPU box](https://raw.githubusercontent.com/amukherjee1991/bpytop-nv/main/docs/screenshots/gpu-only.png)

Each NVIDIA GPU gets its own section with:

- **Graphs**: GPU utilization on top and VRAM usage below, mirrored like the CPU graph.
- **Stats**:
  - GPU and VRAM meters
  - power draw against the power limit
  - temperature with a mini graph, and fan speed
  - graphics and memory clocks
  - video encoder (`ENC`) and decoder (`DEC`) load
  - PCIe receive (▼) and transmit (▲) throughput
  - performance state (`PST`, P0 is the highest)
- **GPU processes** (on terminals at least 110 columns wide): PID, name, type, per-process GPU (SM) utilization and VRAM, sorted by VRAM. In the type column, `C` is compute, `G` is graphics and `B` is both.

Systems with several GPUs get one section per GPU. Data comes straight from NVML, the NVIDIA Management Library, through `ctypes`.

## Views

Press `m` to cycle presets, or toggle single boxes with `1` to `5`.

**`gpu` preset** (cpu, gpu, proc), for watching training or inference jobs:

![gpu preset](https://raw.githubusercontent.com/amukherjee1991/bpytop-nv/main/docs/screenshots/gpu-preset.png)

**Smaller terminals.** On short terminals the GPU box drops to a compact mode, so the boxes below keep their minimum size (100×30 shown):

![compact view](https://raw.githubusercontent.com/amukherjee1991/bpytop-nv/main/docs/screenshots/compact.png)

## Keys

Every bpytop key still works. Press `H` or `F1` for the full list:

![help menu](https://raw.githubusercontent.com/amukherjee1991/bpytop-nv/main/docs/screenshots/help.png)

These keys are new or changed:

| Key | Action |
| --- | --- |
| `5` | Toggle the GPU box |
| `m` | Cycle presets: `full` (cpu gpu mem net proc) → `stat` (cpu gpu mem net) → `proc` (cpu proc) → `gpu` (cpu gpu proc) |

Use `-b` to pick boxes at startup:

```sh
bpytop-nv -b "cpu gpu proc"
```

The choice is saved to the config file when you quit, the same way bpytop does it.

## Configuration

Config, logs and user themes live in `~/.config/bpytop-nv/`, separate from bpytop's `~/.config/bpytop/`. Stock bpytop rejects a config that contains the `gpu` box, so the two can't share one. To choose the default boxes, edit `shown_boxes` in `bpytop-nv.conf`:

```
shown_boxes="cpu gpu mem net proc"
```

bpytop's themes are bundled. Pick one with `o` (options) → `color_theme`.

## Screenshots

The images in [`docs/screenshots/`](docs/screenshots) are captures of the running app on an RTX 3060 and an i7-13700K. The username and LAN address have been replaced.

## Credits and license

- [bpytop](https://github.com/aristocratos/bpytop) by Aristocratos, Apache License 2.0. bpytop-nv is a modified version of it, and its themes are bundled.
- The GPU view takes its ideas from [nvtop](https://github.com/Syllo/nvtop) by Maxime Schmitt. This project contains no nvtop code.

bpytop-nv is licensed under the [Apache License 2.0](LICENSE). See [NOTICE](NOTICE) for attribution and a list of changes.
