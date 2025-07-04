# Space Networking Kit

English | [中文文档](README_ch.md)

A simulation\emulation system for space networking.

*SNK* is a simulation\emulation  platform designed to evaluate the network performance of constellation systems for global Internet services.  SNK offers real-time communication visualization and supports the simulation of routing between edge node of network.  The platform enables the evaluation of routing and network performance metrics such as latency, stretch, network capacity, and throughput under different network structures and density.   The effectiveness of SNK is demonstrated through various simulation cases, including the routing between fixed edge stations or mobile edge stations and analysis of space network structures.

This framework contains:
- **snk-scenario** - Satellite constellation generation and orbital mechanics
- **snk-visualizer** - 3D visualization using CesiumJS
- **snk-server** - Network simulation server
- **snk-analyzer** - Performance analysis tools

## 🚀 Quick Start

### Prerequisites

- **Python 3.8+** with pip
- **Node.js** (optional, for http-server)
- **Git**
- **Linux/WSL** environment recommended

### Installation

1. **Clone the repository**
```bash
git clone <repository-url>
cd snk
```

2. **Install Python dependencies**
```bash
pip install path.py pygeoif pymap3d
```

## 📡 SNK-Scenario: Satellite Constellation Generation

### Setup

1. **Navigate to scenario directory**
```bash
cd snk_scenario
```

2. **Configure constellation parameters**
Edit `configs/config.yaml`:
```yaml
root: "."
dump_path: "./output"
start_time: "2000-01-01T00:00:00" 
end_time: "2000-01-02T00:00:00"
time_step: 100

constellations:
  LAYER0:
    enable: true
    num_orbits: 10        # Number of orbital planes
    num_sats_per_orbit: 10 # Satellites per orbit
    # ... other parameters
```

3. **Run the scenario generator**
```bash
bash run.sh
```

This generates:
- `LAYER0_sats.czml` - Satellite constellation data (~6.3MB)
- `LAYER0_isls.czml` - Inter-satellite links (~91KB) 
- `LAYER0_tle.txt` - Two-Line Element orbital data
- `lump.json` - Configuration metadata
- `config.yaml` - Configuration backup

## 🌍 SNK-Visualizer: 3D Visualization

### Setup

1. **Move generated data to visualizer**
```bash
# From project root
mkdir -p snk_visualizer/data
mv snk_scenario/output/* snk_visualizer/data/
```

2. **Download and setup CesiumJS**
```bash
cd snk_visualizer
wget https://github.com/CesiumGS/cesium/releases/download/1.107/Cesium-1.107.zip
unzip Cesium-1.107.zip
mv Build/Cesium .
cp -r Apps/Sandcastle Cesium/
rm Cesium-1.107.zip
```

3. **Update configuration**
Verify `snk_visualizer/configs/config.yaml` has:
```yaml
base: "../data/"
```

### Running the Visualizer

1. **Start HTTP server**
```bash
cd snk_visualizer
python3 -m http.server 8080
```

2. **Open browser**
Navigate to: **http://localhost:8080/Apps/index.html**

3. **Load constellation**
- Click **`INIT`** button to load satellite data
- Use controls:
  - `Label` - Toggle satellite names
  - `Orbit` - Show orbital paths  
  - `Sensor` - Display coverage areas

## 🛠 Troubleshooting

### Common Issues

**1. Import Errors**
```bash
# ModuleNotFoundError: No module named 'path'
pip install path.py

# ModuleNotFoundError: No module named 'pygeoif'  
pip install pygeoif pymap3d
```

**2. Python Path Issues**
Ensure `run.sh` contains:
```bash
export PYTHONPATH="${PYTHONPATH}:."
```

**3. 404 Errors in Visualizer**
- Use correct URL: `http://localhost:8080/Apps/index.html`
- Not: `http://localhost:8080/index.html`

**4. Data Loading Issues**
- Check `snk_visualizer/configs/config.yaml` has `base: "../data/"`
- Verify data files exist in `snk_visualizer/data/`

**5. Cesium Not Loading**
- Ensure Cesium directory is at `snk_visualizer/Cesium/`
- Check `Cesium.js` file exists and is ~3.8MB

### File Structure
```
snk/
├── snk_scenario/           # Constellation generation
│   ├── scripts_space/      # Core Python scripts
│   ├── configs/           # Configuration files  
│   └── output/            # Generated data (ignored)
├── snk_visualizer/        # 3D visualization
│   ├── Apps/              # Web application
│   ├── Cesium/            # CesiumJS library (ignored)
│   ├── data/              # Scenario data (ignored)
│   └── configs/           # Visualizer config
├── snk_analyzer/          # Analysis tools
├── snk_server/            # Network server
└── snklib/                # Shared libraries
```

## 📊 SNK Workflow

![](./fig/wkfl.png)

## 🏗 SNK Architecture

![](./fig/framework.png)

## 📈 Scenario Examples

![](./fig/sce_abs.png)

![](./fig/har2lon.png)

## 📊 Evaluation Results

![](./fig/cities.png)

![](./fig/loads_thp.png)
![](./fig/stretch_evo.png)

## 🎯 Expected Output

**Constellation Generated:**
- ✅ 100 satellites (10 orbits × 10 satellites)
- ✅ 200 inter-satellite links (100 intra-orbital + 100 inter-orbital)
- ✅ Real-time 3D visualization
- ✅ Interactive controls for labels, orbits, sensors

**Performance Metrics:**
- Latency and network stretch analysis
- Throughput and capacity evaluation  
- Routing performance under different topologies

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open Pull Request

## 📄 Citation

```bibtex
@inproceedings{snk,
title={Space Networking Kit: A Novel Simulation Platform for Emerging LEO Mega-constellations},
author={Xiangtong, Wang and Xiaodong, Han and Menglong, Yang and Songchen, Han and Wei Li}
booktitle={IEEE International Conference on Communications 2024},
year={2024}
}
```

## 📝 License

This project is licensed under the terms specified in the LICENSE file.
