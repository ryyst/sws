# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static weather visualization project that displays temperature data from the Finnish Meteorological Institute (FMI) open data API. The project consists of self-contained HTML files that fetch weather observations and forecasts, displaying them using both Chart.js visualizations and ASCII charts.

## Architecture

The project uses a simple static file approach with two main weather station implementations:

- **saa.html**: Fixed location weather display (Valpperi)
- **valpperi.html**: Dynamic location-based weather display using filename for location detection
- **asciichart.js**: ASCII chart plotting library for terminal-style visualizations

Each HTML file is self-contained with:
- Inlined Chart.js library for graphical charts
- FMI API integration for weather data fetching
- ASCII chart integration for terminal-style output
- Error handling for API failures
- Color-coded day visualization system

## Key Components

### Data Flow
1. Extract location from filename (valpperi.html) or use hardcoded location (saa.html)
2. Fetch observations and forecasts from FMI API using WFS queries
3. Parse XML responses and format data points
4. Display data in both graphical (Chart.js) and ASCII chart formats
5. Show past observations vs future forecasts side by side

### API Integration
- Uses FMI open data WFS service (`https://opendata.fmi.fi/wfs`)
- Queries both `fmi::observations::weather::timevaluepair` and `fmi::forecast::harmonie::surface::point::timevaluepair`
- Configurable timestep (20 minutes) and timesteps (100 points)
- XML response parsing for temperature data extraction

### Visualization
- Chart.js line charts with day-based color coding
- ASCII charts using the asciichart.js library
- Terminal-style output with retro green-on-black styling
- Freezing point reference line at 0°C

## Development

Since this is a static HTML project, no build tools or package managers are used. Files can be opened directly in a browser or served through a simple HTTP server.

To test locally:
```bash
# Using Python's built-in server
python -m http.server 8000

# Using Node.js http-server (if installed)
http-server

# Or simply open files directly in browser
```

The project is designed to work with minimal dependencies - all libraries are either inlined or included as single files.