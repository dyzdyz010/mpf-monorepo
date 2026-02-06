# MPF Monorepo

All MPF components in a single repository for easier debugging.

## Structure

```
mpf-monorepo/
├── sdk/              # Foundation SDK (header-only interfaces)
├── ui-components/    # QML UI component library
├── http-client/      # HTTP client library
├── host/             # Host application
└── plugins/
    ├── orders/       # Sample Orders plugin
    └── rules/        # Sample Rules plugin
```

## Building

### Prerequisites

- Qt 6.8.x with MinGW
- CMake 3.21+

### Build Steps

```bash
# Configure
cmake -B build -G "MinGW Makefiles" -DCMAKE_BUILD_TYPE=Debug

# Build
cmake --build build

# Run
./build/bin/mpf-host
```

### Qt Creator

1. Open `CMakeLists.txt` as project
2. Configure with MinGW kit
3. Build and run

## Output Structure

After build:
```
build/
├── bin/
│   └── mpf-host.exe
├── plugins/
│   ├── orders-plugin.dll
│   └── rules-plugin.dll
└── qml/
    ├── MPF/
    │   ├── Components/
    │   └── Host/
    ├── YourCo/
    │   └── Orders/
    └── Biiz/
        └── Rules/
```

## Debugging

This monorepo build links all components statically (except plugins as DLLs),
making it easier to step through code across component boundaries.

To debug the heap corruption issue, set breakpoints in:
- `host/include/cross_dll_safety.h` - Deep copy functions
- `host/src/navigation_service.cpp` - Route registration and retrieval
- Plugin `start()` methods where routes are registered
