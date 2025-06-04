# Trocadero Control System - UI/UX Design Specification

## Design Philosophy

The Trocadero Control System interface prioritizes **clarity, efficiency, and reliability** for 24/7 operational use. The design emphasizes immediate visual feedback, intuitive controls, and fail-safe operation patterns suitable for high-stress environments.

## Brand Guidelines

### Color Palette
```scss
// Primary Colors (Trocadero Brand)
$trocadero-black: #1a1a1a;
$trocadero-red: #dc143c;
$trocadero-white: #ffffff;

// Functional Colors
$success-green: #28a745;
$warning-orange: #ffc107;
$error-red: #dc3545;
$info-blue: #17a2b8;

// Neutral Colors
$dark-grey: #2d2d2d;
$medium-grey: #6c757d;
$light-grey: #e9ecef;
$off-white: #f8f9fa;

// Status Colors
$online-green: #20c997;
$offline-grey: #6c757d;
$maintenance-blue: #007bff;
$error-red: #dc3545;
```

### Typography
```scss
// Primary Font Stack
$font-primary: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
$font-monospace: 'JetBrains Mono', 'Consolas', monospace;

// Font Weights
$weight-light: 300;
$weight-regular: 400;
$weight-medium: 500;
$weight-semibold: 600;
$weight-bold: 700;

// Font Sizes
$text-xs: 0.75rem;   // 12px
$text-sm: 0.875rem;  // 14px
$text-base: 1rem;    // 16px
$text-lg: 1.125rem;  // 18px
$text-xl: 1.25rem;   // 20px
$text-2xl: 1.5rem;   // 24px
$text-3xl: 1.875rem; // 30px
```

## Layout Structure

### Master Layout
```
┌─────────────────────────────────────────────────────────────┐
│                     Top Navigation Bar                      │
│  [Logo] [Canvas Status] [Alerts] [User Menu] [Emergency]   │
├─────────────────────────────────────────────────────────────┤
│ Side │                                                     │
│ Nav  │                Main Content Area                    │
│ Menu │                                                     │
│      │                                                     │
│ [🏠] │              Dynamic Dashboard Views                │
│ [🎨] │                                                     │
│ [📅] │                                                     │
│ [⚙️] │                                                     │
│ [👥] │                                                     │
├─────────────────────────────────────────────────────────────┤
│                    Status Footer                            │
│      [System Health] [Last Update] [Connected Users]       │
└─────────────────────────────────────────────────────────────┘
```

## Component Specifications

### 1. Canvas Control Panel
```typescript
interface CanvasControlPanelProps {
  canvas: Canvas;
  onStateChange: (state: CanvasState) => void;
  realTimeData: boolean;
  emergencyMode: boolean;
}
```

**Layout:**
```
┌─────────────────────────────────────────────┐
│ 🎭 Exterior Facade Lighting        [🟢 ON] │
├─────────────────────────────────────────────┤
│ Brightness: ████████░░ 80%                  │
│ Color:     [🔴] #DC143C  [Color Picker]     │
│ Pattern:   [Dropdown: Celebration v]        │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ Quick Actions                           │ │
│ │ [Flourish] [Seasonal] [Emergency Stop] │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ Schedule: Next at 18:00 - Evening Show     │
│ [Override] [Edit Schedule] [Preview]        │
└─────────────────────────────────────────────┘
```

### 2. Color Picker Component
```typescript
interface ColorPickerProps {
  value: RGB;
  presets: ColorPreset[];
  onChange: (color: RGB) => void;
  showPreview: boolean;
}
```

**Design:**
```
┌─────────────────────────────────────┐
│ Color Picker                        │
├─────────────────────────────────────┤
│ ┌─────────┐ ┌─────┐                 │
│ │ [Color  │ │RGB: │                 │
│ │  Wheel] │ │R:220│                 │
│ │         │ │G: 20│                 │
│ │    +    │ │B: 60│                 │
│ └─────────┘ └─────┘                 │
│                                     │
│ Presets:                            │
│ [🔴][🟢][🔵][🟡][🟣][⚪][⚫]         │
│                                     │
│ [Apply] [Cancel] [Preview on Wall]  │
└─────────────────────────────────────┘
```

### 3. Content Library Browser
```typescript
interface ContentLibraryProps {
  content: ContentAsset[];
  filters: FilterOptions;
  onSelect: (contentId: string) => void;
  viewMode: 'grid' | 'list';
}
```

**Grid View:**
```
┌─────────────────────────────────────────────┐
│ Content Library                    [🔍][⚙️] │
├─────────────────────────────────────────────┤
│ Filters: [All] [Video] [Images] [Templates] │
│ ┌───────┐ ┌───────┐ ┌───────┐ ┌───────┐     │
│ │[Thumb]│ │[Thumb]│ │[Thumb]│ │[Thumb]│     │
│ │Video1 │ │Image2 │ │Temp3  │ │Video4 │     │
│ │2.3MB  │ │450KB  │ │120KB  │ │5.7MB  │     │
│ │✅Ready│ │✅Ready│ │⏳Proc. │ │✅Ready│     │
│ └───────┘ └───────┘ └───────┘ └───────┘     │
│                                             │
│ [Upload New] [Create Template] [Import]     │
└─────────────────────────────────────────────┘
```

### 4. Schedule Calendar
```typescript
interface ScheduleCalendarProps {
  events: CalendarEvent[];
  view: 'month' | 'week' | 'day';
  onEventClick: (event: CalendarEvent) => void;
  onTimeSlotClick: (time: Date, canvas?: string) => void;
}
```

**Week View:**
```
┌─────────────────────────────────────────────┐
│ June 2025              [Month][Week][Day]   │
├─────────────────────────────────────────────┤
│Time │Mon  │Tue  │Wed  │Thu  │Fri  │Sat │Sun │
├─────┼─────┼─────┼─────┼─────┼─────┼────┼────┤
│09:00│     │     │FADE │     │     │    │    │
│     │     │     │LIGHT│     │     │    │    │
├─────┼─────┼─────┼─────┼─────┼─────┼────┼────┤
│12:00│PROMO│     │     │LUNCH│     │    │    │
│     │VIDEO│     │     │ MSG │     │    │    │
├─────┼─────┼─────┼─────┼─────┼─────┼────┼────┤
│18:00│EVEN │EVEN │EVEN │EVEN │EVEN │WKND│WKND│
│     │SHOW │SHOW │SHOW │SHOW │SHOW │SHOW│SHOW│
└─────┴─────┴─────┴─────┴─────┴─────┴────┴────┘
```

### 5. System Dashboard
```typescript
interface SystemDashboardProps {
  canvases: Canvas[];
  systemHealth: SystemHealth;
  recentActivity: Activity[];
  alerts: Alert[];
}
```

**Dashboard Layout:**
```
┌─────────────────────────────────────────────┐
│ System Overview                             │
├─────────────────────────────────────────────┤
│ Canvas Status:                              │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────┐ │
│ │🟢 Facade    │ │🟢 Canopy    │ │🟡 Stairs│ │
│ │   Online    │ │   Online    │ │  Maint. │ │
│ │   80% Bright│ │   Content#3 │ │  Calib. │ │
│ └─────────────┘ └─────────────┘ └─────────┘ │
│                                             │
│ ┌─────────────┐ ┌─────────────┐             │
│ │🟢 Video Wall│ │🟢 Main Disp │             │
│ │   Interactive│ │   4 Layers  │             │
│ │   3 Users   │ │   Live Feed │             │
│ └─────────────┘ └─────────────┘             │
│                                             │
│ Recent Activity:           Alerts: [2]      │
│ • 11:30 - Celebration triggered on Stairs  │
│ • 11:25 - Content uploaded: promo_video.mp4│
│ • 11:20 - Schedule updated: Evening Show   │
└─────────────────────────────────────────────┘
```

## Responsive Design Patterns

### Breakpoints
```scss
$breakpoint-sm: 576px;   // Small devices
$breakpoint-md: 768px;   // Medium devices (tablets)
$breakpoint-lg: 992px;   // Large devices (desktops)
$breakpoint-xl: 1200px;  // Extra large devices
$breakpoint-xxl: 1400px; // Ultra wide displays
```

### Mobile Adaptations
**Navigation:** Collapsible hamburger menu
**Canvas Controls:** Stacked vertical layout
**Color Picker:** Touch-optimized with larger touch targets
**Calendar:** Simplified day/week view only

## Interaction Patterns

### Real-time Feedback
```typescript
// Visual feedback patterns
interface FeedbackPattern {
  immediate: 'button_press_effect'; // < 50ms
  loading: 'spinner_with_progress'; // operation in progress
  success: 'green_checkmark_flash'; // 2 second display
  error: 'red_shake_animation';     // attention-grabbing
  warning: 'orange_pulse_border';   // requires attention
}
```

### Emergency Controls
```
┌─────────────────────────────────────┐
│ 🚨 EMERGENCY OVERRIDE PANEL         │
├─────────────────────────────────────┤
│ ⚠️  This will override all canvases │
│                                     │
│ Action: [All Black] [Safe Mode v]   │
│ Duration: [Manual Clear] [30 Min v] │
│                                     │
│ Authorization Required:             │
│ Password: [______________]          │
│                                     │
│ [ACTIVATE OVERRIDE] [Cancel]        │
└─────────────────────────────────────┘
```

## Accessibility Standards

### WCAG 2.1 AA Compliance
- **Color Contrast:** Minimum 4.5:1 ratio for normal text
- **Keyboard Navigation:** Full functionality without mouse
- **Screen Reader Support:** Semantic HTML and ARIA labels
- **Focus Indicators:** Clear visual focus states
- **Alternative Text:** All images and icons include alt text

### Accessibility Features
```typescript
// ARIA Labels for Canvas Controls
<button 
  aria-label="Increase brightness for Exterior Facade Lighting"
  aria-describedby="brightness-value"
  role="slider"
  aria-valuemin="0"
  aria-valuemax="100"
  aria-valuenow="80"
>
```

## Animation Guidelines

### Micro-interactions
```scss
// Standard transitions
$transition-fast: 0.15s ease-out;
$transition-normal: 0.3s ease-out;
$transition-slow: 0.5s ease-out;

// Canvas state changes
.canvas-update {
  transition: all $transition-normal;
}

// Success feedback
.success-flash {
  animation: success-pulse 1s ease-out;
}

@keyframes success-pulse {
  0% { box-shadow: 0 0 0 0 rgba(40, 167, 69, 0.4); }
  70% { box-shadow: 0 0 0 10px rgba(40, 167, 69, 0); }
  100% { box-shadow: 0 0 0 0 rgba(40, 167, 69, 0); }
}
```

### Loading States
```
┌─────────────────────────────────────┐
│ Uploading Content...               │
│ ████████████░░░░ 75%               │
│ Processing video: Generating thumb  │
│ ETA: 2 minutes remaining           │
└─────────────────────────────────────┘
```

## Error Handling UI

### Error Message Patterns
```typescript
interface ErrorDisplayProps {
  severity: 'info' | 'warning' | 'error' | 'critical';
  title: string;
  message: string;
  actions?: ErrorAction[];
  dismissible: boolean;
}

// Critical System Error
┌─────────────────────────────────────┐
│ 🔴 CRITICAL: Canvas Connection Lost │
├─────────────────────────────────────┤
│ Exterior Facade Lighting is        │
│ unresponsive. System attempting     │
│ automatic reconnection.             │
│                                     │
│ [Retry Connection] [Emergency Mode] │
│ [Contact Support] [Dismiss]         │
└─────────────────────────────────────┘
```

## Component Library Structure

### Base Components
- `Button` - Primary, secondary, danger variants
- `Input` - Text, number, password, textarea
- `Select` - Dropdown with search capability
- `Slider` - Range input with live preview
- `ColorPicker` - RGB/HSL picker with presets
- `Calendar` - Event scheduling interface
- `ProgressBar` - Upload/processing progress
- `Alert` - System notifications
- `Modal` - Confirmation dialogs
- `Tooltip` - Contextual help

### Composite Components
- `CanvasControlPanel` - Individual canvas management
- `ContentLibrary` - Asset browser and management
- `ScheduleCalendar` - Event planning interface
- `SystemDashboard` - Overview and monitoring
- `EmergencyPanel` - Override controls
- `UserManagement` - Account and permissions
- `UploadManager` - Multi-file upload interface

This UI/UX design specification provides a comprehensive foundation for creating an intuitive, accessible, and reliable control interface that meets the demanding operational requirements of the Trocadero installation while maintaining visual consistency with the brand identity.