# Static Weather Station (SWS)

A minimal, self-contained Finnish weather visualization built on the FMI open data API. Zero dependencies, zero build steps, zero configuration files.

Just one HTML file with suspiciously inlined Chart.js library. Drag & drop anywhere.

## Installation

```sh
wget https://raw.githubusercontent.com/ryyst/sws/refs/heads/main/sws.html && firefox sws.html
```


## Query Parameters

| Parameter | Default   | Range   | Description |
|-----------|-----------|---------|-------------|
| `q`       | `tampere` | Any Finnish location | Location name (fuzzy matched by FMI) |
| `past`    | `12`      | 1-168   | Hours of historical data |
| `future`  | `28`      | 1-168   | Hours of forecast data |
| `theme`   | auto      | `light`\|`dark` | Override system theme |


## Examples

```bash
# Helsinki with extended forecast
?q=helsinki&future=72

# Minimal view for embedded use
?q=oulu&past=6&future=12

# Dark theme override
?q=rovaniemi&theme=dark

# Week-long analysis
?q=turku&past=168&future=168
```
