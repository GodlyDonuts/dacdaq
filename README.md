<div align="center">
  
  <br>

  <h1>
    ⚡️ DacDAQ ⚡️
  </h1>

  <p>
    <b>A robust, high-performance, and scalable system for real-time data acquisition, processing, and visualization.</b>
  </p>

  <p>
    <a href="https://github.com/GodlyDonuts/dacdaq/actions">
      <img src="https://img.shields.io/github/actions/workflow/status/GodlyDonuts/dacdaq/ci.yml?branch=main&style=for-the-badge" alt="Build Status">
    </a>
    <a href="https://opensource.org/licenses/MIT">
      <img src="https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge" alt="License: MIT">
    </a>
    <a href="https://github.com/GodlyDonuts/dacdaq/releases">
      <img src="https://img.shields.io/github/v/release/GodlyDonuts/dacdaq?style=for-the-badge" alt="Latest Release">
    </a>
    <a href="https://github.com/GodlyDonuts/dacdaq/issues">
      <img src="https://img.shields.io/github/issues/GodlyDonuts/dacdaq?style=for-the-badge" alt="Open Issues">
    </a>
  </p>
</div>

---

### <p align="center">DacDAQ is a complete software toolkit for interfacing with high-speed hardware and streaming data to multiple consumers. It's built for applications in scientific research, industrial IoT, and real-time analytics.</p>

<br>

<details>
  <summary><strong>📖 Table of Contents</strong></summary>
  <ol>
    <li><a href="#✨-features">Features</a></li>
    <li>
      <a href="#🚀-getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#▶️-usage">Usage</a></li>
    <li><a href="#🌲-project-structure">Project Structure</a></li>
    <li><a href="#🤝-contributing">Contributing</a></li>
    <li><a href="#📜-license">License</a></li>
    <li><a href="#✉️-contact">Contact</a></li>
  </ol>
</details>

---

## ✨ Features

- ⚡ **High-Throughput:** Capable of handling **over 100 MS/s** (MegaSamples/second) or **1.6 Gb/s** of sustained data flow.
- 🧩 **Modular Architecture:** Easily extend functionality by adding new plugins for hardware (e.g., `NI-DAQmx`, `Red Pitaya`) or data sinks (e.g., `InfluxDB`, `Kafka`, `HDF5`).
- ⏱️ **Real-time Processing:** Apply filters, transformations, and analytics on-the-fly using `NumPy` and `SciPy`.
- 📡 **Multiple Sinks:** Stream data simultaneously to files (HDF5, Parquet), databases, and real-time dashboards.
- 🌐 **Web-Based Monitoring:** Comes with a lightweight `FastAPI` + `Plotly Dash` interface to monitor system status and visualize live data streams.
- **Async-First:** Built with `asyncio` to handle high-concurrency I/O operations without blocking.

---

## 🚀 Getting Started

Get your local copy up and running in a few simple steps.

### Prerequisites

- `Python 3.10+`
- `pip` and `venv`
- `NumPy`, `Pandas`, `FastAPI`
- A compatible DAQ device (e.g., NI-DAQmx) or use the built-in simulated device for testing.

### Installation

```bash
# 1. Clone the repository
git clone [https://github.com/GodlyDonuts/dacdaq.git](https://github.com/GodlyDonuts/dacdaq.git)
cd dacdaq

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`

# 3. Install dependencies
pip install -r requirements.txt

# 4. Set up your local configuration
cp config/default.yml config/local.yml
# ...then edit config/local.yml with your device and database settings
````

-----

## ▶️ Usage

Show users how to use your project with clear code examples.

### Example 1: As a Library

```python
# main.py
from dacdaq import Controller

# 1. Configure the acquisition
config = {
    "device": "Dev1",  # Typical NI-DAQmx device name
    "sample_rate": 1_000_000, # 1 MS/s
    "channels": ["Dev1/ai0", "Dev1/ai1"], # Analog input channels
    "sink": {
        "type": "file", 
        "format": "hdf5",
        "path": "data/session_01.h5"
    }
}

# 2. Start the acquisition
print("Starting acquisition... Press Ctrl+C to stop.")
with Controller(config) as dac:
    try:
        dac.start()
        # Keep the main thread alive while dac runs
        dac.wait() 
    except KeyboardInterrupt:
        print("\nStopping acquisition...")
```

### Example 2: As a Standalone Service

Run the DAQ system as a service with a web monitor.

```bash
# Run the main application server from the root directory
python -m dacdaq.server --config config/local.yml
```

> **Note**
> After starting the service, you can access the monitoring dashboard at `http://localhost:8080`.

-----

## 🌲 Project Structure

A high-level overview of the repository's layout.

```
dacdaq/
├── config/           # Example configuration files (default.yml)
├── dacdaq/           # Main source code package
│   ├── __init__.py
│   ├── server.py     # FastAPI web server entrypoint
│   ├── core/         # Core logic, data pipeline, Controller
│   ├── inputs/       # Hardware/sensor plugins (ni_daq.py, simulator.py)
│   ├── outputs/      # Data sink plugins (hdf5_sink.py, influx_sink.py)
│   └── processing/   # Real-time processing modules (filters.py)
├── docs/             # Project documentation
├── scripts/          # Helper scripts (build, deploy, etc.)
├── tests/            # Unit and integration tests
├── .gitignore
├── LICENSE
├── README.md         # You are here!
└── requirements.txt  # Project dependencies
```

-----

## 🤝 Contributing

Contributions are what make the open-source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1.  **Fork** the Project
2.  **Create your Feature Branch** (`git checkout -b feature/AmazingFeature`)
3.  **Commit your Changes** (`git commit -m 'Add some AmazingFeature'`)
4.  **Push to the Branch** (`git push origin feature/AmazingFeature`)
5.  **Open a Pull Request** against the `develop` branch.

### Branching Strategy

\<details\>
\<summary\>\<strong\>Click to expand our GitFlow Branching Strategy\</strong\>\</summary\>

This project uses a branching model based on **GitFlow**.

  * 🌲 **`main`**: This branch contains production-ready, tagged releases. All code on `main` is stable and deployable. **Do not commit directly to this branch.**
  * 🛠️ **`develop`**: This is the main development branch. It contains the latest "work-in-progress" code. All feature branches are merged into `develop`.
  * ✨ **`feature/your-feature-name`**: Create these branches from `develop` for any new feature.
      * *Example:* `feature/add-hdf5-support`
  * 🐞 **`fix/bug-description`**: Create these branches from `develop` to fix a non-urgent bug.
      * *Example:* `fix/memory-leak-in-streamer`
  * 🔥 **`hotfix/issue-name`**: Create these branches from `main` *only* to fix a critical, production-breaking bug. This branch is merged into both `main` and `develop`.

\</details\>

-----

## 📜 License

Distributed under the **MIT License**. See `LICENSE` file for more information.

-----

## ✉️ Contact

@GodlyDonuts

**Project Link:** [https://github.com/GodlyDonuts/dacdaq](https://github.com/GodlyDonuts/dacdaq)
```
```
