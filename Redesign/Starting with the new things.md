---
tags:
  - frontend
  - best-practices
---
## **Tech Stack**
---
- **Frontend** = Vite & React TS.
- **UI Library** = Shadcn UI
- **State Management** = Zustand
- **Authentication** = PropelAuth
- **Map Visualization** = Maptiler SDK
- **Weather Visualization** = Maptiler Weather API & Windy API 
- **Webcams** = Windy API
- **CSS Utility Libraries** = TailwindCSS

## **Frontend Clean Code Guidelines - Detailed Explanation**
---

## **1. TypeScript Excellence**

### **Why Strict Typing Matters**
In a geospatial application, data consistency is critical. When dealing with coordinates, weather data, and map interactions, type errors can lead to completely wrong visualizations or broken functionality.

**Key Principles:**
- **Readonly Properties**: Prevent accidental mutations of critical data like coordinates and IDs
- **Branded Types**: Distinguish between different types of IDs (StationId vs WebcamId) at compile time
- **Tuple Types**: Enforce proper coordinate format `[longitude, latitude]` instead of ambiguous arrays
- **API Response Contracts**: Ensure external API data matches our internal expectations through transformation layers

**Without this**, you might end up with:
- Coordinates swapped (lat/lng vs lng/lat)
- Invalid station IDs being passed to weather APIs
- Missing data fields causing runtime errors in map components

---

## **2. Zustand State Management for Geospatial Data**

### **Domain-Driven Store Design**
Unlike traditional CRUD apps, mapping applications have unique state management needs:

**Spatial Data Organization:**
- **Maps for Lookups**: Using `Map<StationId, WeatherStation>` instead of arrays provides O(1) lookups and prevents duplicates
- **Bounds-Aware Data**: Stores should only contain data relevant to the current map viewport
- **Layer State Management**: Track which map layers (weather, webcams, etc.) are active

**Performance Optimizations:**
- **Selector Hooks**: Create derived data (like stations in current viewport) without causing unnecessary re-renders
- **Spatial Indexing**: Filter data by geographic bounds at the selector level
- **Background Updates**: Handle real-time data updates without blocking UI interactions

**Common Pitfalls Avoided:**
- Storing entire world's weather data in memory
- Re-rendering all map markers when one station updates
- Mixing UI state with domain data

---

## **3. API Service Layer with Error Handling**

### **The API Gateway Pattern**
With multiple external APIs (Maptiler, Windy, PropelAuth), we need a consistent approach:

**Separation of Concerns:**
- **API Clients**: Isolate external API interactions in dedicated classes
- **Data Transformation**: Convert API-specific formats to our domain models
- **Error Normalization**: Standardize error handling across different APIs

**Why This Matters:**
- **Maintainability**: When Maptiler changes their API, only one file needs updating
- **Testability**: Mock entire API responses by replacing service instances
- **Consistency**: All weather data follows the same structure regardless of source

**Real-World Benefits:**
- Handle API rate limits gracefully
- Provide fallback data when services are down
- Implement retry logic for flaky connections

---
## **4. React Components with Geospatial Data**

### **The Component Hierarchy**
Mapping applications have unique component relationships:

**Layer-Based Architecture:**
```
Map Container
├── Base Map Layer
├── Weather Station Layer
│   ├── Weather Station Marker
│   └── Station Tooltip
├── Webcam Layer
└── Controls Layer
```

**Key Design Patterns:**
- **Container/Presentational**: Separate data-fetching logic from rendering
- **Compound Components**: Group related map elements (Marker + Tooltip)
- **Custom Hooks**: Encapsulate map interactions and data subscriptions

**Performance Considerations:**
- **Virtualization**: Only render markers in current viewport
- **Event Delegation**: Handle map events efficiently rather than individual marker handlers
- **Memoization**: Prevent re-renders of complex map components

---
## **5. Authentication Integration with PropelAuth**

### **Security-First Design**
Weather and mapping data often has tiered access levels:

**Authentication Patterns:**
- **Route Protection**: Prevent unauthorized access to premium features
- **Role-Based Access**: Differentiate between free users, premium users, and admins
- **Token Management**: Automatically refresh auth tokens for long sessions

**User Experience Considerations:**
- **Loading States**: Show appropriate feedback during auth checks
- **Graceful Degradation**: Provide limited functionality for unauthenticated users
- **Error Recovery**: Handle auth failures without losing user data

**Why This Matters:**
- Protect paid API endpoints and premium data layers
- Personalize user experience based on subscription level
- Maintain session state across map interactions

---

## **6. Performance Optimization for Map Data**

### **The Challenge of Real-Time Geospatial Data**
Mapping applications face unique performance challenges:

**Data Volume Issues:**
- Weather stations worldwide = thousands of data points
- Real-time updates = constant data streams
- Map interactions = frequent data refetching

**Solutions Implemented:**

**Debounced Data Fetching:**
- Wait until user stops panning/zooming before fetching new data
- Prevents API overload and UI jank during map navigation
- Configurable delay balances responsiveness vs performance

**Spatial Query Optimization:**
- Only fetch data within current map bounds
- Implement efficient bounds checking algorithms
- Cache previously loaded regions

**Efficient Re-rendering:**
- Use React.memo for expensive map components
- Implement shouldComponentUpdate for custom markers
- Batch state updates to minimize paint operations

---

## **7. Environment Configuration & Security**

### **Production-Ready Configuration**
Geospatial apps have complex configuration needs:

**Security Concerns:**
- API keys exposed in client-side code
- Different configurations per environment (dev/staging/prod)
- Sensitive map data requiring proper access controls

**Best Practices:**
- **Build-Time Configuration**: Use Vite's environment variables for compile-time configuration
- **Type Safety**: Validate environment variables match expected schema
- **Error Early**: Fail fast during build if required configuration is missing

**Why This Matters:**
- Prevent accidental API key exposure
- Ensure consistent behavior across environments
- Provide clear error messages for configuration issues

---

## **8. File Structure & Organization**

### **Domain-Driven Architecture**
Traditional folder-by-type structure doesn't work well for complex applications:

**Problems with Type-Based Organization:**
```
components/
  Button.tsx
  WeatherStation.tsx
  MapMarker.tsx
  WebcamFeed.tsx
```
This becomes unmaintainable as the app grows.

**Benefits of Feature-Based Organization:**
```
components/
  weather/           # Everything weather-related
    WeatherStation.tsx
    TemperatureGauge.tsx
    WeatherLayer.tsx
  webcams/           # Everything webcam-related  
    WebcamFeed.tsx
    WebcamGrid.tsx
  map/               # Map-specific components
    layers/          # Reusable map layers
    controls/        # Map controls
```

**Advantages:**
- **Discoverability**: Find all weather-related code in one place
- **Team Scalability**: Multiple teams can work on different features
- **Reusability**: Shared map components across different features
- **Maintainability**: Changes to weather system are isolated

---

## **Key Architectural Decisions Explained**

### **1. Why Zustand over Redux?**
- **Simplicity**: Less boilerplate for rapidly changing requirements
- **TypeScript Integration**: Excellent native TypeScript support
- **Performance**: Fine-grained subscriptions prevent unnecessary re-renders
- **Bundle Size**: Smaller footprint for better loading performance

### **2. Why Service Classes over Hook-Only APIs?**
- **Separation of Concerns**: Business logic separate from React lifecycle
- **Testability**: Services can be unit tested without React dependency
- **Reusability**: Same service logic works in non-React contexts
- **Dependency Injection**: Easy to mock for testing

### **3. Why Custom Hooks for Everything?**
- **Consistent Patterns**: Uniform approach to side effects and state
- **Composability**: Build complex behavior from simple hooks
- **Testability**: Isolated logic that can be tested independently
- **Reusability**: Share behavior across multiple components

### **4. Why the Focus on Performance?**
- **Map Complexity**: Rendering hundreds of markers with real-time data
- **User Expectations**: Smooth interactions at 60fps
- **Data Costs**: Minimize API calls to reduce costs and improve reliability
- **Mobile Considerations**: Efficient performance on lower-powered devices
---
## **Common Anti-Patterns to Avoid**

1. **Storing all data globally** - Keep only necessary data in stores
2. **Fetching on every render** - Use proper dependency arrays and debouncing
3. **Mixing coordinate systems** - Standardize on [lng, lat] consistently
4. **Ignoring loading states** - Every async operation needs proper feedback
5. **Over-optimizing early** - Measure performance before optimizing

This architecture provides a solid foundation that can scale from a simple weather map to a complex geospatial analytics platform while maintaining code quality and developer productivity.