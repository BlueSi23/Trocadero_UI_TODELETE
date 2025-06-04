# Product Context

This file provides a high-level overview of the project and the expected product that will be created. Initially it is based upon projectBrief.md (if provided) and all other available project-related information in the working directory. This file is intended to be updated as the project evolves, and should be used to inform all other modes of the project's goals and context.
2025-06-04 10:46:19 - Log of updates made will be appended as footnotes to the end of this file.

## Project Goal

Create a comprehensive web-based control infrastructure to manage multiple display canvases, content layers, and real-time interactions across the Trocadero interactive installation venue. The system must provide unified control over 5 distinct canvas systems while supporting real-time interactivity, content management, and external data integration.

## Key Features

* **Multi-Canvas Control System**: Unified management of 5 canvas types (Exterior Facade Lighting, LED Canopy/Sphere/Cube, Interior Stairway LEDs, Entrance Video Wall, Main Stairway Display)
* **Web-Based Dashboard**: Browser-based responsive interface using Trocadero brand colors (black, red, white)
* **Real-Time Interactive Features**: Blob tracking, touch interfaces, celebration triggers, live parameter adjustment
* **Content Management System**: Upload, organization, template creation, approval workflows, version control
* **Scheduling & Automation**: Calendar-based scheduling, recurring events, cross-canvas synchronization
* **External Data Integration**: Casino management system, marketing systems, live streams, API feeds
* **Multi-Layer Content Composition**: Background, framed content, text overlay, and trigger layers
* **Performance Requirements**: <1s response time, <5s content propagation, 99.9% uptime

## Overall Architecture

**Three-Tier Control Architecture:**
1. **Show Control System**: Master scheduling, state management, cross-canvas coordination
2. **Content Management System**: Asset management, template creation, approval workflows
3. **Interactive System**: Sensor integration, real-time response generation, blob tracking

**Canvas Systems:**
- Exterior Facade Lighting (programmable LED with seasonal themes)
- LED Canopy/Sphere/Cube (atmospheric textural content)
- Interior Stairway LEDs (interactive blob tracking, celebration modes)
- Entrance Video Wall (wayfinding, touch interface)
- Main Stairway Display (multi-layer composition, live data integration)

**Development Phases:** 4-phase approach from core infrastructure → advanced content management → interactive features → data integration