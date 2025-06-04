# Trocadero Control System - Technical Architecture

## System Architecture Overview

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    Web Dashboard (Frontend)                     │
│                React + TypeScript + WebSockets                  │
└─────────────────────┬───────────────────────────────────────────┘
                      │ HTTPS/WSS
┌─────────────────────▼───────────────────────────────────────────┐
│                  API Gateway & Load Balancer                    │
│                        Nginx + HAProxy                          │
└─────────────────────┬───────────────────────────────────────────┘
                      │
    ┌─────────────────┼─────────────────┐
    │                 │                 │
┌───▼───┐     ┌───────▼──────┐     ┌────▼────┐
│ Show  │     │   Content    │     │Interactive│
│Control│     │ Management   │     │  System  │
│Service│     │   Service    │     │ Service  │
└───┬───┘     └───────┬──────┘     └────┬────┘
    │                 │                 │
┌───▼─────────────────▼─────────────────▼───┐
│           Shared Database Layer           │
│         PostgreSQL + Redis Cache         │
└───────────────────┬───────────────────────┘
                    │
┌───────────────────▼───────────────────────┐
│              Message Queue                │
│              RabbitMQ/MQTT                │
└───────────────────┬───────────────────────┘
                    │
┌───────────────────▼───────────────────────┐
│            Canvas Controllers             │
│     Hardware Abstraction Layer (HAL)     │
└───┬────┬────┬────┬────┬───────────────────┘
    │    │    │    │    │
┌───▼┐ ┌─▼─┐ ┌▼──┐ ┌▼──┐ ┌▼──────────┐
│LED │ │LED│ │LED│ │Vid│ │Main Stairs│
│Ext │ │Can│ │Int│ │Wall│ │Display    │
└────┘ └───┘ └───┘ └───┘ └───────────┘
```

## Technology Stack Decisions

### Frontend Layer
**Framework:** React 18 with TypeScript
**Rationale:**
- Mature ecosystem with excellent real-time capabilities
- Strong TypeScript support for type safety
- Component-based architecture suitable for modular canvas controls
- Extensive library ecosystem for UI components

**Key Libraries:**
- **State Management:** Zustand (lightweight, performant)
- **Real-time Communication:** Socket.IO client
- **UI Framework:** Material-UI with custom Trocadero theme
- **Color Picker:** react-colorful
- **Calendar/Scheduling:** FullCalendar
- **File Upload:** react-dropzone
- **Charts/Monitoring:** Recharts

### Backend Services
**Framework:** Node.js with Express.js and TypeScript
**Rationale:**
- JavaScript/TypeScript consistency across stack
- Excellent WebSocket support for real-time features
- Strong ecosystem for media and hardware integration
- Fast development cycles

**Microservices Architecture:**
1. **Show Control Service** (Port 3001)
2. **Content Management Service** (Port 3002)
3. **Interactive System Service** (Port 3003)
4. **API Gateway** (Port 3000)

### Database Layer
**Primary Database:** PostgreSQL 15
**Rationale:**
- ACID compliance for critical scheduling data
- JSON support for flexible content metadata
- Robust backup and replication
- Excellent performance for complex queries

**Cache Layer:** Redis 7
**Rationale:**
- Sub-millisecond response times for real-time controls
- Session management
- Real-time data caching
- Pub/Sub for real-time notifications

### Message Queue
**System:** RabbitMQ with MQTT plugin
**Rationale:**
- Reliable message delivery for canvas updates
- MQTT support for IoT devices and sensors
- Dead letter queues for error handling
- Clustering support for high availability

### Hardware Abstraction Layer
**Protocol Support:**
- **DMX512:** For lighting control (Exterior Facade, LED systems)
- **Art-Net/sACN:** For professional lighting networks
- **TCP/UDP:** For media server communication
- **WebRTC:** For camera/sensor data streams
- **MQTT:** For IoT sensor integration

## Service Architecture Details

### Show Control Service
**Responsibilities:**
- Master scheduling across all canvases
- State synchronization
- Cross-canvas coordination
- Emergency override handling

**Key Components:**
```typescript
interface ShowControlService {
  scheduleManager: ScheduleManager;
  stateManager: StateManager;
  synchronizer: CanvasSynchronizer;
  emergencyController: EmergencyController;
}
```

### Content Management Service
**Responsibilities:**
- Asset upload and storage
- Template management
- Content approval workflows
- Version control

**Storage Strategy:**
- **Local Storage:** High-performance NVMe SSD for active content
- **Cloud Backup:** AWS S3 or Azure Blob for archival
- **CDN:** CloudFront for global asset delivery

### Interactive System Service
**Responsibilities:**
- Sensor data processing
- Blob tracking algorithms
- Touch interface management
- Real-time response generation

**Integration Points:**
- Camera systems via RTSP/WebRTC
- Touch controllers via TCP/USB
- Presence sensors via MQTT

## Database Schema Design

### Core Entities

```sql
-- Canvas Management
CREATE TABLE canvases (
    id UUID PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    type VARCHAR(50) NOT NULL,
    hardware_config JSONB,
    status VARCHAR(20) DEFAULT 'active',
    created_at TIMESTAMP DEFAULT NOW()
);

-- Content Management
CREATE TABLE content_assets (
    id UUID PRIMARY KEY,
    filename VARCHAR(255) NOT NULL,
    content_type VARCHAR(100),
    file_size BIGINT,
    metadata JSONB,
    upload_date TIMESTAMP DEFAULT NOW(),
    status VARCHAR(20) DEFAULT 'pending'
);

-- Scheduling System
CREATE TABLE schedules (
    id UUID PRIMARY KEY,
    canvas_id UUID REFERENCES canvases(id),
    content_id UUID REFERENCES content_assets(id),
    start_time TIMESTAMP NOT NULL,
    end_time TIMESTAMP,
    recurrence_rule VARCHAR(255),
    priority INTEGER DEFAULT 1,
    created_by UUID REFERENCES users(id)
);

-- User Management
CREATE TABLE users (
    id UUID PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    role VARCHAR(20) NOT NULL, -- admin, operator, marketing
    permissions JSONB,
    last_login TIMESTAMP,
    created_at TIMESTAMP DEFAULT NOW()
);
```

## Real-Time Communication Architecture

### WebSocket Implementation
```typescript
// Real-time event types
interface RealtimeEvents {
  'canvas:update': CanvasUpdateEvent;
  'content:upload:progress': UploadProgressEvent;
  'system:alert': SystemAlertEvent;
  'celebration:trigger': CelebrationEvent;
  'sensor:data': SensorDataEvent;
}

// Canvas update broadcasting
class CanvasUpdateBroadcaster {
  async broadcastUpdate(canvasId: string, update: CanvasUpdate) {
    await this.redis.publish(`canvas:${canvasId}`, JSON.stringify(update));
    this.websocketServer.to(`canvas:${canvasId}`).emit('canvas:update', update);
  }
}
```

## Security Architecture

### Authentication & Authorization
**Strategy:** JWT with Role-Based Access Control (RBAC)

**User Roles:**
- **Admin:** Full system access, user management
- **Operator:** Canvas control, emergency overrides
- **Marketing:** Content management, scheduling

**Security Measures:**
- HTTPS/WSS encryption for all communications
- API rate limiting (100 requests/minute per user)
- Input validation and sanitization
- Content Security Policy (CSP) headers
- Regular security audits

## Deployment Architecture

### High Availability Setup
```yaml
# docker-compose.yml structure
services:
  nginx:
    image: nginx:alpine
    ports: ["80:80", "443:443"]
  
  api-gateway:
    build: ./api-gateway
    replicas: 2
    
  show-control:
    build: ./services/show-control
    replicas: 2
    
  content-management:
    build: ./services/content-management
    replicas: 2
    
  interactive-system:
    build: ./services/interactive-system
    replicas: 2
    
  postgres:
    image: postgres:15
    volumes: ["postgres_data:/var/lib/postgresql/data"]
    
  redis:
    image: redis:7-alpine
    
  rabbitmq:
    image: rabbitmq:3-management
```

### Infrastructure Requirements
**Minimum Hardware:**
- **CPU:** 8-core Intel/AMD processor
- **RAM:** 32GB DDR4
- **Storage:** 1TB NVMe SSD + 4TB HDD for archival
- **Network:** Gigabit Ethernet with redundant connections

**Monitoring & Logging:**
- **Application Monitoring:** Prometheus + Grafana
- **Log Aggregation:** ELK Stack (Elasticsearch, Logstash, Kibana)
- **Error Tracking:** Sentry
- **Uptime Monitoring:** UptimeRobot

## Performance Optimization

### Response Time Targets
- **Canvas Control:** < 500ms end-to-end
- **Content Upload:** Progressive loading with chunks
- **Real-time Updates:** < 100ms WebSocket latency
- **Database Queries:** < 50ms for standard operations

### Caching Strategy
```typescript
// Multi-level caching
interface CacheStrategy {
  browser: ServiceWorkerCache;    // Static assets
  redis: RedisCache;             // API responses
  application: MemoryCache;      // Computed values
  database: PostgreSQLCache;     // Query results
}
```

This architecture provides the foundation for a robust, scalable, and maintainable control system that meets the Trocadero installation's demanding requirements while ensuring 99.9% uptime and sub-second response times.