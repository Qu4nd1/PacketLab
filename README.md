# PacketLab

A beginner-friendly packet sniffer and analyzer built step by step in Python. PacketLab started as a learning project: the goal is to understand how network traffic really looks on the wire by capturing it, decoding it by hand, and comparing the results with Wireshark.

> **Status:** 🚧 Work in progress. See the [roadmap](#roadmap) for current progress.

---

## ⚠️ Legal & Ethical Notice

Only capture traffic on networks **you own or have explicit permission to monitor**. Capturing other people's traffic may be illegal in your country. This project is for education and does not include any offensive features. Never commit `.pcap` files containing personal data.

---

## Features

- [ ] List available network interfaces
- [ ] Live packet capture with a one-line summary per packet
- [ ] Manual decoding of Ethernet, IPv4, TCP, UDP and ICMP headers
- [ ] Filtering by protocol, IP address and port
- [ ] Session report: packets per protocol, busiest hosts, most used ports
- [ ] Save and read `.pcap` files (compatible with Wireshark)
- [ ] Bonus (pick at least one): DNS query logger, cleartext traffic detection, simple port-scan detection

## Requirements

- Python 3.10+
- [Scapy](https://scapy.net/)
- Administrator/root privileges for live capture
- [Wireshark](https://www.wireshark.org/) (recommended, to verify results)
- Windows only: [Npcap](https://npcap.com/)

A Linux virtual machine is recommended for safe experimentation.

## Installation

```bash
git clone https://github.com/<your-username>/packetlab.git
cd packetlab
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

## Usage

> The commands below describe the **planned** interface and may change during development.

```bash
# List network interfaces
sudo python -m packetlab --list-interfaces

# Capture 50 packets on an interface
sudo python -m packetlab -i eth0 -c 50

# Capture only TCP traffic on port 80
sudo python -m packetlab -i eth0 --protocol tcp --port 80

# Save a capture to a file
sudo python -m packetlab -i eth0 -c 100 -w capture.pcap

# Analyze an existing capture and print a report
python -m packetlab -r capture.pcap --report
```

### Example output (planned)

```
12:01:03.482  192.168.1.12 -> 142.250.74.46   TCP   54 bytes
12:01:03.490  192.168.1.1  -> 192.168.1.12    DNS   89 bytes
```

## Project Structure

```
packetlab/
├── docs/notes/         # Learning notes for each stage
├── samples/            # Anonymized sample captures
├── src/packetlab/
│   ├── cli.py          # Command-line interface
│   ├── capture.py      # Live capture / pcap reading
│   ├── parsers/        # Ethernet, IP, TCP, UDP, ICMP, DNS decoders
│   ├── filters.py      # Packet filtering
│   ├── stats.py        # Statistics and reports
│   └── output.py       # Output formatting
├── tests/
├── SPECIFICATIONS.md   # Project specifications (French)
└── README.md
```

## Roadmap

| Stage | Goal | Status |
|-------|------|--------|
| 0 | Set up environment (Wireshark, Python, Scapy) | ⬜ |
| 1 | Learn the basics: TCP/IP layers, Ethernet, IP, TCP/UDP, ports | ⬜ |
| 2 | Observe real traffic in Wireshark (ping, DNS, HTTP) | ⬜ |
| 3 | First capture script | ⬜ |
| 4 | Decode headers manually from raw bytes | ⬜ |
| 5 | Filtering and statistics | ⬜ |
| 6 | Read and write `.pcap` files | ⬜ |
| 7 | Bonus feature | ⬜ |
| 8 | CLI polish, tests, documentation | ⬜ |

## Running Tests

```bash
pytest
```

## What I Learned

*(To be filled in as the project progresses: key concepts, mistakes, and discoveries for each stage.)*

## Resources

- [Wireshark User's Guide](https://www.wireshark.org/docs/wsug_html_chunked/)
- [Scapy Documentation](https://scapy.readthedocs.io/)
- RFC 791 (IPv4), RFC 793 (TCP), RFC 768 (UDP), RFC 1035 (DNS)

## License

MIT License (to be confirmed, add a `LICENSE` file).
