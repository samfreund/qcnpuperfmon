# qcnpuperfmon

Standalone daemon that polls Qualcomm NPU/DSP performance metrics once per
second and writes them to a file.  Uses the Debian-packaged
[libqcnpuperf](https://tracker.debian.org/pkg/libqcnpuperf) library rather
than building from source.

## Requirements

- Linux on Qualcomm aarch64 (Debian/Ubuntu)
- `libqcnpuperf-dev` and `libqcnpuperf1` installed (from Debian unstable contrib)
- `/dev/fastrpc*` device nodes (FastRPC kernel driver)

## Build

```bash
sudo apt install cmake pkg-config libqcnpuperf-dev libfastrpc-dev

mkdir -p build && cd build
cmake ..
make -j$(nproc)
sudo make install   # installs to /usr/bin/qcnpuperfd + systemd service
```

Or build the `.deb` package directly:

```bash
sudo apt install debhelper cmake pkg-config libqcnpuperf-dev libfastrpc-dev
dpkg-buildpackage -us -uc -b
sudo dpkg -i ../qcnpuperfd_*.deb
```

## Usage

```bash
qcnpuperfd                        # writes to /tmp/qcnpuperf_metrics
qcnpuperfd /run/npu/stats         # custom output path
```

### systemd

```bash
sudo systemctl enable qcnpuperfd
sudo systemctl start qcnpuperfd
journalctl -u qcnpuperfd -f       # follow logs
```

## Output format

Plain text, one metric per line:

```
q6_utilization=<float>    # effective Q6 clock vs max Q6 clock (%)
q6_clock_khz=<uint>      # average Q6 clock frequency in KHz
hvx_utilization=<float>  # HVX utilization vs max Q6 clock (%)
hmx_utilization=<float>  # HMX (NPU matrix engine) utilization vs max Q6 clock (%)
```

## License

BSD-3-Clause. See [LICENSE.txt](LICENSE.txt).
