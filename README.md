# Plugin System

AriaCast features a powerful JavaScript-based plugin system that allows extending the app's UI and functionality without recompiling. This is used to provide native controls for specific servers (like Music Assistant) directly within the AriaCast interface.

### Plugin Storage & Structure
By default, plugins are stored in the app's internal data directory. However, since many Android devices restrict access to `Android/data`, **you can select a custom plugin folder** in the application settings. This allows you to easily manage plugin files using any file explorer.

Each plugin consists of two files:
- `plugin_name.json`: Metadata (ID, name, version, author).
- `plugin_name.js`: The JavaScript logic executed via the Rhino engine.

### JavaScript API Reference
Plugins have access to several global objects to interact with the Android system and AriaCast service:

#### `ui` (UI Operations)
- `ui.run(function)`: Runs code on the Android UI thread.
- `ui.add(view)`: Adds an Android View to the plugin container.
- `ui.clear()`: Removes all views from the plugin container.
- `ui.inflate(layoutName)`: Inflates a native XML layout (e.g., `item_server`).
- `ui.findView(parent, idName)`: Finds a view within an inflated layout by its ID string.

#### `events` (Lifecycle & State)
- `events.onServiceConnected(callback)`: Triggered when connected to an AriaCast server. Provides the service object.
- `events.onStateChanged(callback)`: Triggered on casting state changes (`CONNECTING`, `CASTING`, `IDLE`).
- `events.onConfigRequested(callback)`: Triggered when the user taps the "Settings" icon for the plugin.

#### `http` & `ws` (Networking)
- `http.fetch(url, headers)`: Synchronous GET request.
- `http.post(url, body, headers)`: Synchronous POST request.
- `ws.request(url, command, args, token)`: Specialized WebSocket RPC helper for real-time communication.

#### `storage` (Persistence)
- `storage.get(key)`: Retrieves a string value.
- `storage.set(key, value)`: Persists a string value.

#### `bg` (Concurrency)
- `bg.run(function)`: Executes a function in a background thread to avoid blocking the UI.
