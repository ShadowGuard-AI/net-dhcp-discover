![Banner](banner.png)

# net-dhcp-discover
Performs DHCP discovery to identify DHCP servers on the network. Uses scapy for packet crafting and analysis. Focused on Basic network operations.

## Install
```bash
git clone https://github.com/ShadowGuard-AI/net-dhcp-discover.git
cd net-dhcp-discover
pip install -r requirements.txt
```

Python 3 plus scapy (see `requirements.txt`). Scapy is imported at startup, so the tool cannot even print help without it installed. Sending and sniffing raw packets also needs the usual packet-capture privileges on the host.

## Usage
```bash
python3 main.py [-i INTERFACE] [-t SECONDS] [-v]
```

## Parameters
- `-i`, `--interface`: Network interface to use, for example `eth0` or `wlan0`. If not given, the tool tries scapy's default interface.
- `-t`, `--timeout`: Seconds to wait for DHCP responses. Default: 5. Must be a positive integer.
- `-v`, `--verbose`: Enable verbose output for debugging.
- `-h`, `--help`: Show help message and exit.

## Exit codes
- `0`: the discovery ran, including a run that finds no servers (it prints "No DHCP servers found").
- `1`: no interface could be determined, a non-positive timeout, or a socket or unexpected error during discovery.

## Limits, as the code states them
- The discover packet uses a fixed client hardware address, not the sending interface's MAC, so a server that only answers the real client address will ignore it.
- Sniffing starts only after the discover packet is sent, so a very fast offer can arrive in the gap and be missed, and replies are not matched by transaction id: the capture filter takes any traffic to UDP port 68.
- An offer is recognised only when its first DHCP option is message type 2, and servers are deduplicated by source IP alone.

Verified in Wave 12: `python3 -m py_compile main.py` passes and the parameter list above matches the argument parser; a live run is blocked here by the missing scapy package, and installs are forbidden in this wave, so no live discovery is claimed.

## License
MIT. See [LICENSE](LICENSE).
