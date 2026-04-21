# Frontend - F1 Telemetry & AI Race Engineer Dashboard

## Overview

This is the React/Next.js frontend for the F1 Telemetry Dashboard. It provides:
- Interactive telemetry visualization with Recharts
- AI-generated race engineer analysis display
- F1 session selection interface
- Real-time communication with FastAPI backend

## Project Structure

```
frontend/
├── src/
│   ├── components/
│   │   ├── TelemetryChart.jsx      # Speed, throttle, brake visualization
│   │   ├── AIDriverSummary.jsx     # AI analysis display
│   │   ├── SessionSelector.jsx     # Session selection controls
│   │   └── index.js                # Component exports
│   ├── services/
│   │   └── api.js                  # Backend API client
│   ├── App.jsx                     # Main application component
│   ├── index.js                    # React entry point
│   └── index.css                   # Global styles
├── public/
│   └── index.html                  # HTML template
├── package.json                    # Dependencies & scripts
├── .env.example                    # Environment template
├── tailwind.config.js              # Tailwind CSS config
├── postcss.config.js               # PostCSS config
└── README.md                       # This file
```

## Setup Instructions

### 1. Install Node Dependencies

```bash
cd frontend
npm install
```

This installs:
- React 18
- Recharts (charting library)
- Tailwind CSS (styling)
- Axios (HTTP client)
- Development tools

### 2. Configure Environment

```bash
# Copy environment template
copy .env.example .env.local

# Edit .env.local with your settings
```

**Key Configuration:**
```
REACT_APP_API_URL=http://localhost:8000/api
REACT_APP_DEBUG=true
```

### 3. Start Development Server

```bash
npm start
```

Frontend will open at: `http://localhost:3000`

The app will automatically reload when you make changes.

## Available Scripts

### Development
```bash
npm start              # Start development server with hot reload
npm run build          # Create optimized production build
npm test               # Run test suite
npm run eject          # Eject from Create React App (irreversible)
```

### Code Quality
```bash
npm run lint           # Run ESLint
npm run format         # Format code with Prettier
```

## Environment Variables

Create `.env.local` with:

```
# Required
REACT_APP_API_URL=http://localhost:8000/api

# Optional
REACT_APP_DEBUG=true
REACT_APP_ENV=development
REACT_APP_ENABLE_MOCK_DATA=true
REACT_APP_ENABLE_AI_FEATURES=true
```

## Component Documentation

### TelemetryChart Component
Displays F1 telemetry data as interactive charts.

**Props:**
- `telemetryData` (array) - Telemetry data points with distance, speed, gear, etc.
- `driverName` (string) - Driver name for chart header

**Features:**
- Toggle speed, throttle, brake, gear visualization
- Custom tooltip on hover
- Responsive container sizing
- Mock data generation for demo

**Usage:**
```jsx
import TelemetryChart from './components/TelemetryChart';

<TelemetryChart 
  telemetryData={data} 
  driverName="Max Verstappen" 
/>
```

### AIDriverSummary Component
Displays AI-generated race engineer analysis.

**Props:**
- `driverA` (number) - First driver number
- `driverB` (number) - Second driver number
- `event` (string) - Grand Prix name
- `year` (number) - Season year

**Features:**
- Fetches analysis from backend
- Displays performance comparison
- Shows tire management, brake analysis
- Key insights & recommendations
- Risk assessment

**Usage:**
```jsx
import AIDriverSummary from './components/AIDriverSummary';

<AIDriverSummary 
  driverA={1} 
  driverB={11} 
  event="Bahrain" 
  year={2024}
/>
```

### SessionSelector Component
Allows users to select F1 season, event, and session.

**Props:**
- `onSessionSelect` (function) - Callback when session is selected

**Features:**
- Season selector (2020-2025)
- Grand Prix dropdown
- Session type selector
- Quick preset buttons
- Session summary display

**Usage:**
```jsx
import SessionSelector from './components/SessionSelector';

const handleSessionSelect = (session) => {
  console.log(session); 
  // { year: 2024, event: 'Bahrain', sessionType: 'R' }
};

<SessionSelector onSessionSelect={handleSessionSelect} />
```

## API Service

The `api.js` module provides:

### Telemetry API
```javascript
import { api } from './services/api';

// Get session telemetry
api.telemetry.getSessionTelemetry(2024, 'Bahrain', 'R');

// Get driver lap data
api.telemetry.getDriverLapTelemetry(1, 5);

// Compare drivers
api.telemetry.compareDrivers(1, 11);
```

### AI Analysis API
```javascript
// Generate analysis
api.ai.analyzeRaceEngineers(1, 11, { year: 2024, event: 'Bahrain' });

// Compare telemetry with AI
api.ai.compareTelemetry(1, 11, telemetryA, telemetryB);

// Get analysis history
api.ai.getAnalysisHistory(1);

// Get event insights
api.ai.getEventInsights(2024, 'Bahrain');
```

### Health Check
```javascript
api.health(); // Returns { status: 'healthy', ... }
```

## Styling

### Tailwind CSS
Styling uses Tailwind utility classes. Configuration in `tailwind.config.js`:

```javascript
module.exports = {
  content: ['./src/**/*.{js,jsx}'],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### Common Classes Used
- `flex`, `grid` - Layout
- `text-*` - Font sizes & styles
- `bg-*` - Background colors
- `border-*` - Borders
- `rounded-*` - Border radius
- `shadow-*` - Shadows
- `hover:*` - Hover states
- `responsive prefixes` - Mobile responsiveness

## Development Tips

### Hot Reload
Changes automatically reload the browser. To disable, remove `-reload` from package.json scripts.

### Mock Data
TelemetryChart uses generated mock data if no `telemetryData` prop provided.

### Debug Mode
Set `REACT_APP_DEBUG=true` in `.env.local` to enable:
- Verbose console logging
- Component debug info
- API request/response logs

### Browser DevTools
- React Developer Tools extension recommended
- Redux DevTools for state management (if added)

## Common Issues

### "Cannot GET /api/telemetry/..."
- Ensure backend is running on `http://localhost:8000`
- Check `REACT_APP_API_URL` in `.env.local`
- Verify CORS is enabled in backend

### "Module not found: recharts"
```bash
npm install recharts
```

### Port 3000 already in use
```bash
# Find and kill process on port 3000
netstat -ano | findstr :3000
taskkill /PID <PID> /F
```

### Blank page on load
1. Check browser console for errors
2. Verify `.env.local` exists and has correct API URL
3. Try hard refresh: Ctrl+Shift+R
4. Check backend is running

## Performance Optimization

### Code Splitting
React automatically code-splits at route level.

### Lazy Loading
```jsx
const TelemetryChart = React.lazy(() => import('./components/TelemetryChart'));
```

### Image Optimization
Use next/image (if using Next.js) or optimize images before import.

### Caching
API responses can be cached in localStorage:
```javascript
const cached = localStorage.getItem('telemetry_data');
```

## Testing

### Run Tests
```bash
npm test
```

### Example Test
```javascript
import TelemetryChart from './components/TelemetryChart';
import { render } from '@testing-library/react';

it('renders telemetry chart', () => {
  const { getByText } = render(
    <TelemetryChart driverName="Test Driver" />
  );
  expect(getByText('Test Driver')).toBeInTheDocument();
});
```

## Building for Production

```bash
npm run build
```

Creates optimized build in `build/` directory:
- Minified JavaScript
- Optimized images
- CSS purged of unused styles
- Source maps (for debugging)

### Deployment
The build folder is ready to deploy:

```bash
# Serve locally to test
npm install -g serve
serve -s build
```

## Future Features

- [ ] User authentication
- [ ] Save favorite analyses
- [ ] Custom telemetry filtering
- [ ] Advanced comparison tools
- [ ] Real-time WebSocket updates
- [ ] Dark mode theme
- [ ] Progressive Web App (PWA)
- [ ] Mobile optimization
- [ ] Offline support
- [ ] Data export (PDF, CSV)

## Browser Support

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## Support

For issues:
1. Check console for error messages: F12 → Console
2. Verify backend is running and accessible
3. Check `.env.local` configuration
4. Try hard refresh: Ctrl+Shift+R
5. Clear node_modules: `rm -rf node_modules && npm install`

---

**Frontend Ready!** 🎨
