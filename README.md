# bpytop-nv

[bpytop](https://github.com/aristocratos/bpytop) with an [nvtop](https://github.com/Syllo/nvtop)-style GPU box for NVIDIA cards. You get CPU, GPU, memory, disks, network and processes in one terminal view.

```
┌─┤⁵gpu├────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ GPU 91%                                         ⣤⣤                                    Pid: Program:        T  GPU%   VRAM │
│                                                 ⣿⣿ ┌─┤0:RTX 3060├─────────────────────────┐  416857 llama-server    C    92     9G │
│                                                 ⣿⣿ │GPU ■■■■■■■■■■■■■■■■■■■■■■■  91%      │  110962 code            G     0   724M │
│                                                 ⣿⣿ │MEM ■■■■■■■■■■■■■■■■■■■■■■■ 11.3/12G  │    5039 gnome-shell     G     0   650M │
│                                                 ⣿⣿ │PWR ■■■■■■■■■■■■■■■■■■■■■■■ 108/170W  │   36362 claude-desktop  B     0   122M │
│                                                 ⣿⣿ │TMP ⡀⡀⡀⣤⣤  59°C   FAN 58%             │   38220 chrome          B     0   117M │
│                                                 ⣿⣿ │CLK 1942 MHz   MEM 7301 MHz           │    4961 gnome-remote-d  B     0   106M │
│                                                 ⣿⣿ │ENC   0%   DEC   0%                   │   65984 nautilus        G     0    92M │
│                                                 ⣿⣿ │PCIe ▼ 12G/s    ▲ 1G/s                │    5184 Xwayland        G     0     7M │
│                                                 ⣿⣿ │PST P2   PROCS 9                      │                                    │
│ VRAM 94%                                        ⠿⠿ └──────────────────────────────────────┘                                    │
└───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

## What the GPU box shows

Each NVIDIA GPU gets its own section with:

- **Graphs**: GPU utilization on top and VRAM usage below, mirrored like the CPU graph.
- **Stats**: GPU and VRAM meters, power draw against the power limit, temperature with a mini graph, fan speed, graphics and memory clocks, encoder and decoder load, PCIe throughput and performance state.
- **GPU processes** (on terminals at least 110 columns wide): PID, name, type, per-process SM utilization and VRAM, sorted by VRAM. In the type column, `C` is compute, `G` is graphics and `B` is both.

Systems with several GPUs get one section per GPU. Data comes straight from `libnvidia-ml`, the NVML library that ships with the NVIDIA driver, through `ctypes`. You don't need `pynvml` or `nvidia-smi`. If NVML isn't available, the GPU box stays hidden and everything else works like stock bpytop.

## Requirements

- Linux
- Python 3.7+
- [psutil](https://github.com/giampaolo/psutil) (`pip install psutil` or `sudo apt install python3-psutil`)
- The NVIDIA proprietary driver for the GPU box (`libnvidia-ml.so.1`)

## Install and run

```sh
git clone https://github.com/amukherjee1991/bpytop-nv.git
cd bpytop-nv
./bpytop-nv.py
```

To install it for your user:

```sh
install -Dm755 bpytop-nv.py ~/.local/bin/bpytop-nv
```

Themes are found in the same places bpytop looks: `/usr/[local/]share/bpytop/themes` and `~/.config/bpytop-nv/themes`.

## Keys

Every bpytop key still works (`H` or `F1` shows the full list). These are new or changed:

| Key | Action |
| --- | --- |
| `5` | Toggle the GPU box |
| `m` | Cycle presets: `full` (cpu gpu mem net proc) → `stat` (cpu gpu mem net) → `proc` (cpu proc) → `gpu` (cpu gpu proc) |

Use `-b` to pick boxes at startup:

```sh
./bpytop-nv.py -b "cpu gpu proc"
```

## Configuration

Config and logs live in `~/.config/bpytop-nv/`, separate from bpytop's `~/.config/bpytop/`. Stock bpytop rejects a config that contains the `gpu` box, so the two can't share one. To choose the default boxes, edit `shown_boxes` in `bpytop-nv.conf`:

```
shown_boxes="cpu gpu mem net proc"
```

## Credits and license

- [bpytop](https://github.com/aristocratos/bpytop) by Aristocratos, Apache License 2.0. bpytop-nv is a modified version of it.
- The GPU view takes its ideas from [nvtop](https://github.com/Syllo/nvtop) by Maxime Schmitt. This project contains no nvtop code.

bpytop-nv is licensed under the [Apache License 2.0](LICENSE). See [NOTICE](NOTICE) for attribution and a list of changes.
