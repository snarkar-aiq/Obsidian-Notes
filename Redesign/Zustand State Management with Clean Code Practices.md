## **1. Core Principles for Zustand Stores**

### **Single Responsibility per Store**
**Why:** Each store should manage one domain concept. This prevents bloated stores and makes testing easier.

```typescript
// ✅ GOOD: Separate stores by domain
const useWeatherStore = create<WeatherStore>(...); // Manages weather data
const useMapStore = create<MapStore>(...);         // Manages map state
const useUIStore = create<UIStore>(...);          // Manages UI state
const useAuthStore = create<AuthStore>(...);      // Manages authentication

// ❌ BAD: Monolithic store
const useAppStore = create<AppStore>(...); // Manages everything - becomes unmaintainable
```

### **Immutability with TypeScript**
**Why:** Prevent accidental mutations and ensure predictable state updates.

```typescript
// Store state interfaces should be readonly
interface WeatherState {
  readonly stations: ReadonlyMap<StationId, WeatherStation>;
  readonly loading: boolean;
  readonly lastUpdated: Date | null;
}

// In actions, always return new state
const useWeatherStore = create<WeatherStore>((set, get) => ({
  stations: new Map(),
  
  // ✅ GOOD: Return new state
  updateStation: (id: StationId, updates: Partial<WeatherStation>) => 
    set((state) => ({
      stations: new Map(state.stations).set(id, { 
        ...state.stations.get(id)!, 
        ...updates 
      })
    })),
    
  // ❌ BAD: Mutating existing state
  updateStationBad: (id: StationId, updates: Partial<WeatherStation>) => {
    const station = get().stations.get(id);
    if (station) {
      Object.assign(station, updates); // MUTATION!
      set({ stations: get().stations }); // This won't trigger updates
    }
  }
}));
```

---

## **2. Store Structure and Organization**

### **Separate State, Actions, and Selectors**
**Why:** Clear separation makes stores easier to read, test, and maintain.

```typescript
// types/weather-store.ts
export interface WeatherState {
  readonly stations: ReadonlyMap<StationId, WeatherStation>;
  readonly selectedStationId: StationId | null;
  readonly loading: boolean;
  readonly error: string | null;
}

export interface WeatherActions {
  readonly fetchStations: (bounds: MapBounds) => Promise<void>;
  readonly selectStation: (id: StationId | null) => void;
  readonly updateStation: (id: StationId, updates: Partial<WeatherStation>) => void;
  readonly clearError: () => void;
}

export type WeatherStore = WeatherState & WeatherActions;

// Selectors as separate hooks
export const useStations = () => 
  useWeatherStore((state) => Array.from(state.stations.values()));
  
export const useSelectedStation = () =>
  useWeatherStore((state) => 
    state.selectedStationId ? state.stations.get(state.selectedStationId) : null
  );
```

### **File Organization**
```
src/
  stores/
    weather-store/
      index.ts           // Main store export
      types.ts           // State/action interfaces
      selectors.ts       // All selector hooks
      actions.ts         // Complex action logic (if needed)
    map-store/
      index.ts
      types.ts
      selectors.ts
    ui-store/
      index.ts
      types.ts
    index.ts             // Barrel exports
```

---

## **3. Async Actions Pattern**

### **Proper Loading and Error States**
**Why:** Consistent async handling prevents race conditions and provides better UX.

```typescript
const useWeatherStore = create<WeatherStore>((set, get) => ({
  stations: new Map(),
  loading: false,
  error: null,
  
  fetchStations: async (bounds: MapBounds) => {
    // ✅ Prevent concurrent requests for same data
    if (get().loading) return;
    
    set({ loading: true, error: null });
    
    try {
      const stations = await weatherApi.getStationsInBounds(bounds);
      const stationsMap = new Map(stations.map(s => [s.id, s]));
      
      set({ 
        stations: stationsMap, 
        loading: false,
        lastUpdated: new Date() 
      });
      
    } catch (error) {
      // ✅ Consistent error handling
      const message = error instanceof Error ? error.message : 'Failed to fetch stations';
      set({ 
        error: message, 
        loading: false 
      });
      
      // ✅ Optional: Re-throw for component-level handling
      throw error;
    }
  },
}));
```

### **Optimistic Updates for Better UX**
**Why:** Make the UI feel faster by updating immediately, then handling errors.

```typescript
const useFavoritesStore = create<FavoritesStore>((set, get) => ({
  favorites: new Set(),
  
  toggleFavorite: async (stationId: StationId) => {
    const { favorites } = get();
    const wasFavorite = favorites.has(stationId);
    
    // ✅ Optimistic update
    const newFavorites = new Set(favorites);
    if (wasFavorite) {
      newFavorites.delete(stationId);
    } else {
      newFavorites.add(stationId);
    }
    set({ favorites: newFavorites });
    
    try {
      // ✅ Sync with server
      await api.updateFavorites(Array.from(newFavorites));
    } catch (error) {
      // ✅ Rollback on error
      set({ favorites });
      
      // ✅ Show error notification
      useNotificationStore.getState().showError(
        `Failed to ${wasFavorite ? 'remove from' : 'add to'} favorites`
      );
    }
  },
}));
```

---

## **4. Selector Patterns for Performance**

### **Memoized Selectors**
**Why:** Prevent unnecessary re-renders by computing derived state efficiently.

```typescript
// stores/weather-store/selectors.ts
import { shallow } from 'zustand/shallow';

// ✅ Basic selector
export const useStations = () => 
  useWeatherStore((state) => Array.from(state.stations.values()));

// ✅ Memoized complex selector
export const useStationsInBounds = (bounds: MapBounds | null) => 
  useWeatherStore(
    (state) => bounds 
      ? Array.from(state.stations.values()).filter(s => isInBounds(s, bounds))
      : [],
    shallow // ✅ Prevents re-renders when array reference changes but content doesn't
  );

// ✅ Computed values
export const useStationStats = () =>
  useWeatherStore((state) => {
    const stations = Array.from(state.stations.values());
    return {
      total: stations.length,
      withAlerts: stations.filter(s => s.hasAlerts).length,
      averageTemp: stations.reduce((sum, s) => sum + s.temperature, 0) / stations.length,
    };
  });
```

### **Parameterized Selectors**
**Why:** Create reusable selectors that accept parameters.

```typescript
// ✅ Parameterized selector factory
export const useStationById = (id: StationId | null) =>
  useWeatherStore((state) => id ? state.stations.get(id) : null);

// ✅ Complex parameterized selector
export const useStationsByType = (type: StationType, bounds?: MapBounds) =>
  useWeatherStore((state) => {
    let stations = Array.from(state.stations.values());
    
    if (bounds) {
      stations = stations.filter(s => isInBounds(s, bounds));
    }
    
    return stations.filter(s => s.type === type);
  });
```

---

## **5. Cross-Store Communication**

### **Avoid Circular Dependencies**
**Why:** Keep stores independent and testable.

```typescript
// ✅ GOOD: Composed actions
const useMapStore = create<MapStore>((set, get) => ({
  viewState: initialViewState,
  
  moveToStation: (stationId: StationId) => {
    const station = useWeatherStore.getState().stations.get(stationId);
    if (station) {
      // Update map position
      set({
        viewState: {
          latitude: station.coordinates[1],
          longitude: station.coordinates[0],
          zoom: 12
        }
      });
      
      // Update weather store
      useWeatherStore.getState().selectStation(stationId);
    }
  },
}));

// ❌ BAD: Direct store imports causing circular dependencies
// Don't import stores in other store files
```

### **Using Middleware for Cross-Cutting Concerns**
**Why:** Handle logging, persistence, and analytics consistently.

```typescript
// middleware/persistence.ts
export const persist = (config: StateCreator<Store>) => 
  (set: any, get: any, api: any) => 
    config(
      (args: any) => {
        set(args);
        // Auto-save to localStorage
        localStorage.setItem('store', JSON.stringify(get()));
      },
      get,
      api
    );

// middleware/logger.ts
export const logger = (config: StateCreator<Store>) => 
  (set: any, get: any, api: any) =>
    config(
      (args: any) => {
        console.log('State update:', args);
        set(args);
      },
      get,
      api
    );

// Store with middleware
const useAppStore = create<AppStore>()(
  logger(
    persist((set, get) => ({
      // ... store config
    }))
  )
);
```

---

## **6. Testing Strategies**

### **Test Stores in Isolation**
**Why:** Ensure store logic works correctly without React.

```typescript
// __tests__/weather-store.test.ts
describe('WeatherStore', () => {
  beforeEach(() => {
    // Reset store to initial state
    useWeatherStore.setState(useWeatherStore.getInitialState());
  });

  it('should fetch stations and update state', async () => {
    const mockStations = [createMockStation()];
    vi.spyOn(weatherApi, 'getStationsInBounds').mockResolvedValue(mockStations);
    
    await useWeatherStore.getState().fetchStations(mockBounds);
    
    expect(useWeatherStore.getState().stations.size).toBe(1);
    expect(useWeatherStore.getState().loading).toBe(false);
  });

  it('should handle errors gracefully', async () => {
    vi.spyOn(weatherApi, 'getStationsInBounds').mockRejectedValue(new Error('API failed'));
    
    await expect(
      useWeatherStore.getState().fetchStations(mockBounds)
    ).rejects.toThrow('API failed');
    
    expect(useWeatherStore.getState().error).toBe('API failed');
    expect(useWeatherStore.getState().loading).toBe(false);
  });
});
```

### **Test Selectors**
```typescript
// __tests__/weather-store-selectors.test.ts
describe('WeatherStore selectors', () => {
  beforeEach(() => {
    const stations = new Map([
      ['1', { id: '1', temperature: 20, coordinates: [0, 0] }],
      ['2', { id: '2', temperature: 25, coordinates: [1, 1] }],
    ]);
    useWeatherStore.setState({ stations });
  });

  it('should filter stations by bounds', () => {
    const bounds = { north: 0.5, south: -0.5, east: 0.5, west: -0.5 };
    const stations = useStationsInBounds(bounds);
    
    expect(stations).toHaveLength(1);
    expect(stations[0].id).toBe('1');
  });
});
```

---

## **7. Real-World Complete Example**

### **Map Store Implementation**
```typescript
// stores/map-store/index.ts
interface MapState {
  readonly viewState: {
    readonly latitude: number;
    readonly longitude: number;
    readonly zoom: number;
  };
  readonly bounds: MapBounds | null;
  readonly activeLayers: ReadonlySet<MapLayer>;
  readonly isLoadingTiles: boolean;
}

interface MapActions {
  readonly setViewState: (viewState: Partial<MapState['viewState']>) => void;
  readonly setBounds: (bounds: MapBounds) => void;
  readonly toggleLayer: (layer: MapLayer) => void;
  readonly setLoadingTiles: (loading: boolean) => void;
  readonly moveToStation: (station: WeatherStation) => void;
}

export type MapStore = MapState & MapActions;

const initialState: MapState = {
  viewState: {
    latitude: 40.7128,
    longitude: -74.0060,
    zoom: 10,
  },
  bounds: null,
  activeLayers: new Set(['weather-stations', 'webcams']),
  isLoadingTiles: false,
};

export const useMapStore = create<MapStore>((set, get) => ({
  ...initialState,
  
  setViewState: (updates) => set((state) => ({
    viewState: { ...state.viewState, ...updates }
  })),
  
  setBounds: (bounds) => set({ bounds }),
  
  toggleLayer: (layer) => set((state) => {
    const newLayers = new Set(state.activeLayers);
    if (newLayers.has(layer)) {
      newLayers.delete(layer);
    } else {
      newLayers.add(layer);
    }
    return { activeLayers: newLayers };
  }),
  
  setLoadingTiles: (isLoadingTiles) => set({ isLoadingTiles }),
  
  moveToStation: (station) => {
    const [lng, lat] = station.coordinates;
    get().setViewState({
      latitude: lat,
      longitude: lng,
      zoom: 12, // Zoom in when selecting a station
    });
    
    // Coordinate with weather store
    useWeatherStore.getState().selectStation(station.id);
  },
}));
```

### **Using Stores in Components**
```typescript
// components/MapContainer.tsx
export function MapContainer() {
  // ✅ Use specific selectors to prevent unnecessary re-renders
  const { viewState, activeLayers } = useMapStore();
  const stations = useStationsInBounds(useMapStore(s => s.bounds));
  const selectedStation = useSelectedStation();
  
  const handleStationClick = useCallback((station: WeatherStation) => {
    // ✅ Use actions directly
    useMapStore.getState().moveToStation(station);
  }, []);
  
  return (
    <Map 
      viewState={viewState}
      onMove={useMapStore.getState().setViewState}
    >
      {activeLayers.has('weather-stations') && (
        <WeatherStationsLayer 
          stations={stations}
          selectedStation={selectedStation}
          onStationClick={handleStationClick}
        />
      )}
    </Map>
  );
}
```

## **Key Clean Code Rules for Zustand:**

1. **One domain per store** - Keep stores focused
2. **Immutable updates only** - Always return new state
3. **Separate state, actions, selectors** - Clear architecture
4. **Use selectors for derived data** - Performance optimization
5. **Handle async actions properly** - Loading states and error handling
6. **Test stores in isolation** - Ensure business logic correctness
7. **Avoid cross-store imports** - Prevent circular dependencies
8. **Use middleware for cross-cutting concerns** - Consistent behavior

This approach ensures your Zustand stores are maintainable, performant, and testable as your weather mapping application grows in complexity.