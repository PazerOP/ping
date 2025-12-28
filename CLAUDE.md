# Latency Monitor

A single-page web application for real-time network latency monitoring with visual graphs.

## Architecture

This is a minimal, self-contained HTML file with embedded CSS and JavaScript. No build tools or dependencies.

## Key Features

- **Ping via image loading**: Uses `Image()` object to ping target endpoints (favicon.ico files) since direct ICMP is not available in browsers
- **Fixed interval timeline**: Data points are positioned at evenly-spaced intervals regardless of when responses arrive
- **Smooth bezier interpolation**: Uses cardinal spline curves with tension=0.3 for smooth line rendering between data points
- **Off-screen data arrival**: New data points enter ~2 intervals off-screen to the right and scroll in smoothly
- **Animated transitions**: Y-values and scale smoothly animate using lerp interpolation
- **Packet loss visualization**: Failed pings shown as red X marks at bottom of graph

## State Structure

```javascript
state = {
    samples: [],        // { value: number|null, slot: number }
    displayValues: [],  // Animated Y values for rendering
    displayMaxVal: 100, // Animated Y scale
    pingCount: 0,       // Slot counter for fixed interval positioning
    startTime: null,    // When monitoring started
    maxSamples: 100     // Visible window size
}
```

## Graph Rendering

- X position: `slot * interval` relative to elapsed time (not actual timestamps)
- Y position: Normalized against animated max scale
- Clipping: Canvas clipping used for clean edges
- Frame rate: Uses `requestAnimationFrame` for 60fps smooth scrolling

## Browser Compatibility

Includes tap-to-cycle behavior for select elements for Tesla browser compatibility.
