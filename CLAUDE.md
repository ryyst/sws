# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static Weather Station (SWS) is a minimal, self-contained Finnish weather visualization built on the FMI open data API. The core philosophy is radical simplicity: one HTML file with zero dependencies, zero build steps, zero configuration files.

## Current Implementation

**Primary file**: `sws.html` - Complete weather station application
**Supporting files**: `README.md`, `CLAUDE.md`

The application features:
- Inlined Chart.js library for visualization
- FMI API integration with intelligent parameter handling
- URL-based configuration via query parameters
- Dark/light theme system with browser preference detection
- Responsive design with CSS custom properties
- Canvas-based wind direction arrows and chart overlays

## Architecture & Data Flow

1. **Query Parameter Parsing**: Extract location (`q`), time ranges (`past`, `future`), theme (`theme`)
2. **API Optimization**: Dynamic timestep selection (10min→30min→60min) based on total time range
3. **Dual API Calls**: Separate observation/forecast requests to FMI WFS service
4. **XML Processing**: Direct DOM parsing with namespace handling and NaN value cleanup
5. **Data Normalization**: PoP interpolation, time alignment, missing value handling
6. **Visualization**: Chart.js dual-axis charts + custom plugin overlays

## FMI API Integration

- **Base URL**: `https://opendata.fmi.fi/wfs`
- **Queries**: `fmi::observations::weather::timevaluepair`, `fmi::forecast::harmonie::surface::point::timevaluepair`
- **Parameters**: `temperature`, `Precipitation1h`, `PoP`, `WindSpeedMS`, `WindDirection`
- **Time Handling**: ISO 8601 timestamps with `starttime`/`endtime` ranges
- **Error Handling**: Graceful degradation for missing data, API failures, parsing errors

## Query Parameters

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| `q` | `tampere` | Any Finnish location | Location name (fuzzy matched by FMI) |
| `past` | `12` | 1-168 | Hours of historical data |
| `future` | `28` | 1-168 | Hours of forecast data |
| `theme` | auto | `light`\|`dark` | Override system theme |

## Code Structure & Patterns

### Configuration Constants
- API endpoints and query types
- Default time ranges and timestep thresholds
- DOM element references

### Query Parameter Helpers
- Input validation with proper bounds checking
- Location sanitization and normalization
- Theme detection with browser preference fallback

### API Functions
- Unified parameter building with observation/forecast variants
- HTTP error handling with detailed logging
- XML parsing with error detection

### Data Processing
- Modular parsing functions with clear separation of concerns
- Station information extraction from XML metadata  
- Time-based sorting and missing value interpolation

### Visualization
- CSS custom properties for consistent theming
- Chart.js configuration with responsive design
- Custom plugin system for overlay elements (NOW line, wind arrows, precipitation %)

## Development Guidelines

### Code Quality
- Use modern JavaScript (ES6+) features consistently
- Implement proper error handling and input validation
- Follow existing naming conventions and code organization
- Add JSDoc comments for complex functions

### Chart.js Integration
- **NEVER** set `maintainAspectRatio: false` (causes infinite growth)
- Use inline plugin registration, not global `Chart.register()`
- Access CSS variables via `getCSSVariable()` helper for theme consistency
- Keep chart configuration responsive with proper padding

### Theme System
- All colors must use CSS custom properties (variables)
- Support both light/dark variants with `[data-theme="dark"]` selector
- Apply theme via JavaScript with `document.documentElement.setAttribute()`
- Theme detection: URL param → browser preference → light default

### Error Handling Best Practices
- Validate API responses and XML parsing
- Provide fallback values for missing weather data
- Log errors with context (location, query type, etc.)
- Return empty arrays/objects rather than throwing on failures

### Performance Considerations
- Cache URL parameters and computed values
- Use efficient XML parsing with namespace-aware queries
- Minimize DOM manipulations during chart updates
- Optimize timestep based on total data range

## Testing

```bash
# Local development server
python -m http.server 8000

# Test different configurations
http://localhost:8000/sws.html?q=helsinki&future=72&theme=dark
```

## File Organization

```
/
├── sws.html          # Main application (self-contained)
├── README.md         # User-facing documentation
└── CLAUDE.md         # Developer guidance (this file)
```

Keep the single-file philosophy - all functionality should remain in `sws.html` unless absolutely necessary to separate concerns.