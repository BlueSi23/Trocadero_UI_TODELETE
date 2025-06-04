# Trocadero Control System - API Specification

## API Overview

The Trocadero Control System exposes RESTful APIs for CRUD operations and WebSocket APIs for real-time communication. All APIs follow OpenAPI 3.0 specification and include comprehensive error handling.

## Authentication

### JWT Token Structure
```typescript
interface JWTPayload {
  userId: string;
  username: string;
  role: 'admin' | 'operator' | 'marketing';
  permissions: string[];
  exp: number;
  iat: number;
}
```

### Authentication Endpoints

#### POST /auth/login
```typescript
// Request
interface LoginRequest {
  username: string;
  password: string;
}

// Response
interface LoginResponse {
  token: string;
  refreshToken: string;
  user: {
    id: string;
    username: string;
    role: string;
    permissions: string[];
  };
  expiresIn: number;
}
```

#### POST /auth/refresh
```typescript
// Request
interface RefreshRequest {
  refreshToken: string;
}

// Response - Same as LoginResponse
```

## Canvas Control APIs

### Canvas Management

#### GET /api/canvases
```typescript
// Response
interface CanvasListResponse {
  canvases: Canvas[];
  total: number;
}

interface Canvas {
  id: string;
  name: string;
  type: 'facade' | 'canopy' | 'interior_stairs' | 'entrance_wall' | 'main_stairs';
  status: 'online' | 'offline' | 'maintenance' | 'error';
  currentState: CanvasState;
  lastUpdate: string;
  hardwareConfig: HardwareConfig;
}
```

#### GET /api/canvases/{canvasId}
```typescript
// Response
interface CanvasDetailResponse {
  canvas: Canvas;
  activeContent: ContentAsset | null;
  scheduledContent: ScheduledContent[];
  recentActivity: Activity[];
}
```

#### PUT /api/canvases/{canvasId}/state
```typescript
// Request
interface CanvasStateUpdateRequest {
  state: CanvasState;
  priority: number; // 1-10, 10 = emergency override
  duration?: number; // seconds, null = indefinite
  triggeredBy: string; // user ID or system
}

// Canvas-specific state interfaces
interface FacadeLightingState {
  brightness: number; // 0-100
  color: {
    r: number; // 0-255
    g: number; // 0-255
    b: number; // 0-255
  };
  pattern: string;
  transitionSpeed: number; // 0-100
  specialEffect?: 'flourish' | 'celebration' | 'seasonal';
}

interface InteriorStairsState {
  brightness: number;
  baseColor: RGB;
  interactivityEnabled: boolean;
  sensitivity: number; // 0-100
  effectRadius: number; // pixels
  celebrationMode: boolean;
}

interface MainStairsState {
  layers: {
    background: LayerState;
    framed: LayerState;
    text: LayerState;
    trigger: LayerState;
  };
}
```

### Content Management APIs

#### POST /api/content/upload
```typescript
// Multipart form request
interface ContentUploadRequest {
  file: File;
  metadata: {
    title: string;
    description?: string;
    tags: string[];
    canvasTypes: string[];
    category: 'video' | 'image' | 'template' | 'lighting_sequence';
  };
}

// Response
interface ContentUploadResponse {
  contentId: string;
  status: 'uploading' | 'processing' | 'ready' | 'error';
  uploadProgress: number; // 0-100
  processingStage?: string;
}
```

#### GET /api/content
```typescript
// Query parameters
interface ContentQueryParams {
  category?: string;
  canvasType?: string;
  tags?: string;
  search?: string;
  page?: number;
  limit?: number;
  sortBy?: 'created' | 'name' | 'size';
  sortOrder?: 'asc' | 'desc';
}

// Response
interface ContentListResponse {
  content: ContentAsset[];
  pagination: {
    total: number;
    page: number;
    limit: number;
    pages: number;
  };
  filters: {
    categories: string[];
    canvasTypes: string[];
    availableTags: string[];
  };
}
```

#### PUT /api/content/{contentId}/deploy
```typescript
// Request
interface ContentDeployRequest {
  canvasId: string;
  layer?: string; // for multi-layer canvases
  startTime?: string; // ISO 8601, immediate if not provided
  duration?: number; // seconds
  priority: number;
}

// Response
interface ContentDeployResponse {
  deploymentId: string;
  status: 'queued' | 'deploying' | 'active' | 'completed' | 'failed';
  estimatedStartTime: string;
}
```

### Scheduling APIs

#### POST /api/schedules
```typescript
// Request
interface CreateScheduleRequest {
  name: string;
  canvasId: string;
  contentId: string;
  startTime: string; // ISO 8601
  endTime?: string; // ISO 8601
  recurrence?: {
    rule: string; // RRULE format
    exceptions?: string[]; // ISO 8601 dates
  };
  priority: number;
  enabled: boolean;
}

// Response
interface ScheduleResponse {
  scheduleId: string;
  nextExecution: string;
  status: 'active' | 'paused' | 'expired';
}
```

#### GET /api/schedules/calendar
```typescript
// Query parameters
interface CalendarQueryParams {
  start: string; // ISO 8601
  end: string; // ISO 8601
  canvasId?: string;
  view: 'month' | 'week' | 'day';
}

// Response
interface CalendarResponse {
  events: CalendarEvent[];
  conflicts: ScheduleConflict[];
}

interface CalendarEvent {
  id: string;
  title: string;
  start: string;
  end: string;
  canvasName: string;
  contentName: string;
  priority: number;
  status: 'scheduled' | 'active' | 'completed' | 'cancelled';
}
```

### Interactive System APIs

#### GET /api/interactive/sensors
```typescript
// Response
interface SensorStatusResponse {
  sensors: SensorStatus[];
  systemHealth: {
    overall: 'healthy' | 'warning' | 'critical';
    lastUpdate: string;
    activeAlerts: Alert[];
  };
}

interface SensorStatus {
  id: string;
  name: string;
  type: 'camera' | 'proximity' | 'touch' | 'environmental';
  status: 'online' | 'offline' | 'error';
  lastData: string;
  calibrationStatus: 'calibrated' | 'needs_calibration' | 'calibrating';
  location: {
    canvas: string;
    zone: string;
    coordinates?: { x: number; y: number; z?: number };
  };
}
```

#### POST /api/interactive/calibrate/{sensorId}
```typescript
// Request
interface CalibrationRequest {
  calibrationType: 'zone_mapping' | 'sensitivity' | 'background_subtraction';
  parameters: Record<string, any>;
}

// Response
interface CalibrationResponse {
  calibrationId: string;
  status: 'started' | 'in_progress' | 'completed' | 'failed';
  progress: number; // 0-100
  estimatedCompletion: string;
}
```

### Emergency & Override APIs

#### POST /api/emergency/override
```typescript
// Request
interface EmergencyOverrideRequest {
  canvasIds: string[]; // empty array = all canvases
  action: 'blackout' | 'safe_mode' | 'emergency_message' | 'evacuation';
  message?: string; // for emergency_message action
  duration?: number; // seconds, null = manual clear required
  authorizedBy: string;
}

// Response
interface EmergencyOverrideResponse {
  overrideId: string;
  status: 'active';
  affectedCanvases: string[];
  clearMethod: 'automatic' | 'manual';
  expiresAt?: string;
}
```

#### DELETE /api/emergency/override/{overrideId}
```typescript
// Request
interface ClearOverrideRequest {
  authorizedBy: string;
  reason: string;
}

// Response
interface ClearOverrideResponse {
  status: 'cleared';
  clearedAt: string;
  restoredStates: Record<string, CanvasState>;
}
```

## WebSocket API Specification

### Connection & Authentication
```typescript
// Connection URL: ws://localhost:3000/ws?token={jwt_token}

interface WebSocketMessage {
  type: string;
  payload: any;
  timestamp: string;
  messageId: string;
}
```

### Real-time Events

#### Canvas State Updates
```typescript
// Server -> Client
interface CanvasUpdateEvent {
  type: 'canvas:state:update';
  payload: {
    canvasId: string;
    previousState: CanvasState;
    newState: CanvasState;
    triggeredBy: string;
    priority: number;
  };
}

// Client -> Server (Canvas Control)
interface CanvasControlMessage {
  type: 'canvas:control';
  payload: {
    canvasId: string;
    command: 'brightness' | 'color' | 'pattern' | 'emergency_stop';
    parameters: Record<string, any>;
    immediate: boolean;
  };
}
```

#### Content Upload Progress
```typescript
// Server -> Client
interface UploadProgressEvent {
  type: 'content:upload:progress';
  payload: {
    contentId: string;
    stage: 'uploading' | 'processing' | 'thumbnail_generation' | 'validation';
    progress: number; // 0-100
    estimatedCompletion?: string;
    error?: string;
  };
}
```

#### Sensor Data Streaming
```typescript
// Server -> Client
interface SensorDataEvent {
  type: 'sensor:data';
  payload: {
    sensorId: string;
    dataType: 'blob_tracking' | 'touch' | 'proximity' | 'environmental';
    data: SensorData;
    processed: boolean;
  };
}

interface BlobTrackingData {
  detectedObjects: Array<{
    id: string;
    x: number; // 0-1 normalized
    y: number; // 0-1 normalized
    width: number;
    height: number;
    confidence: number; // 0-1
    velocity?: { dx: number; dy: number };
  }>;
  timestamp: string;
  cameraId: string;
}
```

#### System Alerts
```typescript
// Server -> Client
interface SystemAlertEvent {
  type: 'system:alert';
  payload: {
    severity: 'info' | 'warning' | 'error' | 'critical';
    category: 'hardware' | 'network' | 'content' | 'security' | 'performance';
    message: string;
    details?: Record<string, any>;
    requiresAction: boolean;
    affectedSystems: string[];
  };
}
```

#### Celebration Triggers
```typescript
// Server -> Client & Client -> Server
interface CelebrationEvent {
  type: 'celebration:trigger';
  payload: {
    celebrationType: 'jackpot' | 'big_win' | 'special_event' | 'custom';
    intensity: 1 | 2 | 3 | 4 | 5; // 1 = subtle, 5 = spectacular
    duration: number; // seconds
    data?: {
      winAmount?: number;
      gameType?: string;
      playerTier?: string;
      customMessage?: string;
    };
    affectedCanvases: string[];
  };
}
```

## Error Handling

### HTTP Status Codes
- **200** - Success
- **201** - Created
- **400** - Bad Request (validation errors)
- **401** - Unauthorized (invalid/expired token)
- **403** - Forbidden (insufficient permissions)
- **404** - Not Found
- **409** - Conflict (schedule conflicts, canvas busy)
- **429** - Too Many Requests (rate limit exceeded)
- **500** - Internal Server Error
- **503** - Service Unavailable (hardware offline)

### Error Response Format
```typescript
interface ErrorResponse {
  error: {
    code: string;
    message: string;
    details?: Record<string, any>;
    timestamp: string;
    requestId: string;
  };
  validationErrors?: Array<{
    field: string;
    message: string;
    value: any;
  }>;
}
```

## Rate Limiting

### Limits by User Role
- **Admin**: 1000 requests/minute
- **Operator**: 500 requests/minute  
- **Marketing**: 200 requests/minute

### Endpoint-Specific Limits
- **Canvas Control**: 60 requests/minute per canvas
- **Content Upload**: 10 concurrent uploads per user
- **Emergency Override**: 5 requests/minute (higher priority processing)

## API Versioning

All APIs include version in the URL path: `/api/v1/`

Breaking changes require new version increments. Non-breaking changes (new optional fields, new endpoints) maintain version compatibility.

This API specification provides comprehensive coverage of all system functionality while maintaining clear separation of concerns and robust error handling for the demanding real-time requirements of the Trocadero installation.