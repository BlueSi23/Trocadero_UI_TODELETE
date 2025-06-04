# Trocadero Interactive Installation - Development Project Brief

## Project Overview
The Trocadero interactive installation requires a comprehensive control infrastructure to manage multiple display canvases, content layers, and real-time interactions across the venue. This brief outlines the technical requirements for developing the control interface system based on the project meeting of 25/06/03.

## Canvas Architecture

### 1. Exterior Facade Lighting System
**Hardware:** Architectural LED lighting fixtures

**Control Requirements:**
- Programmable lighting states with custom sequences
- Colour control system (RGB values, preset colour schemes)
- Brightness adjustment (0-100% dimming)
- On/off control with soft transitions
- Scheduling system for automated operation
- Special effect "flourishes" and celebratory modes
- Seasonal/event-based colour themes (e.g., green for St. Patrick's Day)

**Content Type:** Lighting sequences, architectural highlighting patterns

**User Interface Elements:**
- Colour picker with preset palette
- Brightness slider
- State selection dropdown
- Schedule calendar interface
- Manual trigger buttons for special effects

### 2. LED Canopy/Sphere/Cube Installation
**Hardware:** Overhead LED installation (format TBC - sphere, cube, or canopy)

**Control Requirements:**
- Content management system for textural content updates
- Template-based content system
- State synchronisation with facade lighting
- Content scheduling and playlist management

**Content Type:** Atmospheric textural content, ambient lighting patterns

**User Interface Elements:**
- Template library browser
- Content upload interface
- Preview system
- Synchronisation controls with other canvases

### 3. Interior Stairway LED System
**Hardware:** LED domes, pixel panels, or similar diffused LED elements

**Control Requirements:**
- Multiple programmed content states
- Interactive blob tracking integration
- Real-time brightness and colour adjustment
- Content layering system for multiple simultaneous effects
- Triggered celebration modes (jackpot wins, special events)
- Gentle, passive interactivity responding to movement

**Content Type:** Low-resolution, low-poly style content, atmospheric lighting

**Interactive Features:**
- Blob tracking for presence detection
- Soft area-of-effect lighting that follows people
- Wide berth detection (not requiring precise positioning)
- Gentle brightening and colour changes in response to movement

**User Interface Elements:**
- Interactive sensitivity adjustment
- Effect area configuration
- Celebration trigger manual controls
- Real-time movement display/monitoring

### 4. Entrance Video Wall
**Hardware:** Video display with interactive capability

**Control Requirements:**
- Content scheduling system
- Colour scheme control to match facade lighting
- Wayfinding content management
- Interactive layer management
- Video content updates via media server

**Content Type:** Video content, wayfinding information, messaging

**Interactive Features:** Touch/gesture interface for wayfinding

**User Interface Elements:**
- Video content library
- Wayfinding content editor
- Colour synchronisation controls
- Interactive element configuration

### 5. Main Stairway Display System
**Hardware:** Large format display screens

**Control Requirements:**
- Multi-layer content composition system
- Picture-in-picture functionality
- Dynamic text overlay system
- Template-based content management
- Real-time data integration capabilities
- Live stream integration
- Triggered celebration content
- Framed content windows for promotional material

**Content Layers:**
- **Background Layer:** Immersive backdrop content that complements architecture
- **Framed Content Layer:** Promotional windows, live streams, marketing content
- **Text Overlay Layer:** Dynamic messaging, real-time information, celebratory text
- **Trigger Layer:** Celebration content activated by casino events

**User Interface Elements:**
- Layer management system
- Text input interface with font and positioning controls
- Frame positioning and sizing tools
- Live stream source selection
- Real-time data feed configuration

## Control System Architecture

### Show Control System
**Primary Functions:**
- Master scheduling across all canvases
- State management and synchronisation
- Manual trigger capabilities for events
- API integration for external data sources
- Cross-canvas coordination for unified experiences

**Interface Requirements:**
- Master control dashboard
- Global scheduling calendar
- Emergency override controls
- System status monitoring
- Cross-canvas synchronisation controls

### Content Management System
**Primary Functions:**
- Video content upload, storage, and organisation
- Template creation and editing
- Asset library management
- Content approval workflows
- Version control for content updates

**Interface Requirements:**
- Content library browser with search and filter
- Upload interface with progress indication
- Template editor with drag-and-drop functionality
- Preview system for content testing
- Approval workflow interface

### Interactive System
**Primary Functions:**
- Blob tracking and people detection
- Touch interface management
- QR code integration for gamification features
- Real-time response generation
- Sensor data processing and interpretation

**Interface Requirements:**
- Sensor calibration interface
- Interactive zone configuration
- Response behaviour adjustment
- Real-time sensor data visualisation
- Debugging and testing tools

## User Interface Specifications

### Web Dashboard Requirements
**Platform:** Browser-based responsive web application
**Colour Scheme:** Black, red, and white (Trocadero brand colours)

**Core Interface Elements:**

#### System Overview Dashboard
- Real-time status of all canvases
- Active content display
- System health monitoring
- Quick access controls

#### Content Management Interface
- Asset library with thumbnail previews
- Upload interface with drag-and-drop
- Content categorisation and tagging
- Search and filter functionality

#### Scheduling Interface
- Calendar view for content scheduling
- Timeline view for sequence programming
- Recurring event setup
- Holiday/special event configuration

#### Control Panels per Canvas
- Canvas-specific control interfaces
- Real-time parameter adjustment
- Manual override capabilities
- Testing and preview functions

#### Text Messaging System
- Quick text input for dynamic overlays
- Font and positioning controls
- Message scheduling
- Emergency messaging capability

#### System Administration
- Power management controls
- Cleaning/maintenance modes
- User access management
- System log viewing

### Real-time Control Features

#### Parameter Adjustment
- Colour picker with live preview
- Brightness sliders with immediate feedback
- Speed controls for animated content
- Transition duration settings

#### Manual Triggers
- Celebration mode activation
- Emergency content override
- Special event triggers
- Cross-canvas synchronisation triggers

#### Data Integration
- Casino jackpot/winning data feeds
- Website/RSS content integration
- Live stream source management
- External API data display

## Technical Integration Requirements

### API Integration Specifications

**External Data Sources:**
- Casino management system (jackpot data, floor information)
- Website content management system
- Marketing department asset systems
- Building management systems

**Data Types:**
- Real-time winning amounts and locations
- Marketing messages and promotional content
- Event schedules and special occasions
- Occupancy and traffic data

### Hardware Interface Requirements

**Media Server Integration:**
- Support for multiple media server platforms
- Real-time rendering capability
- Pixel mapping for non-standard display configurations
- Content layer compositing

**Sensor Integration:**
- Blob tracking camera systems
- Touch interface hardware
- QR code scanning capability
- Environmental sensors (occupancy, ambient light)

## Development Phases

### Phase 1: Core Infrastructure
- Basic web dashboard framework
- Canvas identification and connection
- Simple content upload and playback
- Basic scheduling functionality

### Phase 2: Advanced Content Management
- Multi-layer content system
- Template-based content creation
- Real-time text overlay system
- Cross-canvas synchronisation

### Phase 3: Interactive Features
- Blob tracking integration
- Touch interface implementation
- Real-time parameter adjustment
- Celebration trigger system

### Phase 4: Data Integration
- External API connections
- Real-time data display
- Advanced scheduling features
- Analytics and reporting

## Success Criteria

### Operational Requirements
- Simple, intuitive interface requiring minimal training
- Reliable 24/7 operation with minimal maintenance
- Flexible content management for marketing team use
- Real-time responsiveness for interactive features

### Performance Metrics
- < 1 second response time for manual controls
- < 5 second content update propagation
- 99.9% uptime for critical display functions
- Support for simultaneous multi-user access

### User Experience Goals
- Marketing team can update content independently
- Facility staff can manage daily operations
- Emergency overrides accessible within 3 clicks
- Visual feedback for all user actions

This development brief provides the foundation for creating a comprehensive control system that meets the operational needs of the Trocadero interactive installation while providing flexibility for future enhancements and expansions.