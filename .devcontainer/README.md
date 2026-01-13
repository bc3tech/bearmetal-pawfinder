# Pawfinder DevContainer Setup

This directory contains the development container configuration for the Pawfinder Flutter project. It enables a consistent, reproducible development environment with all necessary tools pre-configured.

## Quick Start

1. **Install Dependencies**:
   - [Docker Desktop](https://www.docker.com/products/docker-desktop)
   - [Visual Studio Code](https://code.visualstudio.com/)
   - [Remote - Containers Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

2. **Open in DevContainer**:
   - Open the Pawfinder project in VS Code
   - Press `F1` and select **"Dev Containers: Reopen in Container"**
   - Wait for the container to build and initialize (this may a while on first run)

3. **Start Developing**:
   - Flutter dependencies will automatically be fetched during container setup
   - Use VS Code's task runner to build, run, and test

## Available VS Code Tasks

Access tasks via `Ctrl+Shift+B` (or `Cmd+Shift+B` on macOS):

### Dependency Management

- **Flutter: Get Dependencies** - Fetch pubspec dependencies
- **Dart: Pub Upgrade** - Upgrade all dependencies to latest versions

### Code Quality

- **Flutter: Analyze** - Run Dart analyzer for code issues
- **Flutter: Format** - Format code according to Dart style guidelines
- **Flutter: Test** - Run all unit and widget tests

### Build & Run

- **Flutter: Run (Debug)** - Run app in debug mode (default)
- **Flutter: Run (Release)** - Run optimized release build
- **Flutter: Run (Web - Chrome)** - Run web version in Chrome
- **Flutter: Run (Android Emulator)** - Run on Android emulator/device
- **Flutter: Build Web** - Create production web build
- **Flutter: Build APK (Android)** - Create Android APK
- **Flutter: Build AAB (Android)** - Create Android App Bundle

### Maintenance

- **Flutter: Clean** - Remove build artifacts
- **Flutter: Doctor** - Check Flutter environment status

## Debugging

### Debug Configurations

Available via the Debug menu (`Ctrl+Shift+D` / `Cmd+Shift+D`):

1. **Flutter: Debug (Current Device)** - Standard debug on attached device/emulator
2. **Flutter: Debug Web** - Debug web version with full browser DevTools
3. **Flutter: Profile Mode** - Performance profiling without optimizations
4. **Flutter: Release Mode** - Debug release build (limited debugging capability)
5. **Dart: Run Tests** - Run tests with verbose output

### Hot Reload

While a debug session is active:

- Press `r` in the terminal to hot reload
- Press `R` for full app restart
- Changes to `main.dart` or state management may require full restart

### Breakpoints & Inspection

- Click line numbers to set breakpoints
- Hover over variables to see values
- Use Debug Console for expression evaluation
- Full Dart DevTools integration available in browser

## VS Code Extensions

The devcontainer automatically installs a curated set of extensions optimized for Flutter development and best practices:

- **Riverpod Snippets** - State management patterns
- **Error Lens** - Inline error detection
- **GitLens** - Git history and blame
- **Flutter Snippets** - Common widget patterns
- **Version Lens** - Dependency version checking
- **Code Spell Checker** - Documentation quality
- And more...

See [EXTENSIONS.md](./EXTENSIONS.md) for a complete guide to each extension, its purpose, and best practices it facilitates.

## Advanced Usage

### Running Multiple Tasks

You can run multiple development tasks simultaneously:

1. Run `Flutter: Run (Debug)` to start the app
2. Open another terminal in VS Code (`Ctrl+Shift+` ` `)
3. Run `Flutter: Analyze` or other tasks in the new terminal

### Port Forwarding

The devcontainer forwards these ports:

- **8080**: Web development server
- **8888**: DevTools/debugging ports

### Customizing the Environment

Edit `.devcontainer/devcontainer.json` to:

- Add more VS Code extensions
- Modify environment variables
- Change Dart/Flutter versions
- Add custom post-create commands

### Mounting Android/Gradle Caches

The devcontainer mounts your host's Android and Gradle caches to speed up builds:

- `~/.android/` → container `/root/.android`
- `~/.gradle/` → container `/root/.gradle`

This persists build caches and Android SDK downloads across container rebuilds.

## Troubleshooting

### Container Won't Start - Windows WSL Path Issues

If you see errors like `bind source path does not exist: /run/desktop/mnt/host/...`:

**Issue**: Docker Desktop on Windows with WSL can have path mounting issues when the workspace is on a Windows drive.

**Solution Options**:
1. **Already Fixed**: The devcontainer now uses docker-compose.yml which handles Windows paths correctly - just rebuild
2. **Move workspace to WSL** (Better Performance): Copy the project to your WSL filesystem:
   ```bash
   # In WSL terminal
   cp -r /mnt/d/gh/me/bear-metal/pawfinder ~/projects/
   # Then open ~/projects/pawfinder in VS Code
   ```
3. **Verify Docker Desktop Settings**:
   - Open Docker Desktop
   - Go to Settings → Resources → WSL Integration
   - Ensure your WSL distro (Ubuntu) is enabled
   - Restart Docker Desktop if needed

### Container Won't Start - General

```bash
# Rebuild the container from scratch
Dev Containers: Rebuild Container
```

### Flutter Doctor Shows Issues

```bash
# Inside the container terminal, run:
flutter doctor -v
```

### Cache Issues

```bash
# Clear all caches and rebuild
flutter clean
flutter pub get
```

### Android Build Failures

Ensure your Android/Gradle caches are properly mounted:

```bash
ls -la ~/.android/
ls -la ~/.gradle/
```

### Port Already in Use

If ports 8080 or 8888 are already bound:

1. Edit `.devcontainer/devcontainer.json`
2. Modify the `forwardPorts` array with different port numbers
3. Rebuild the container

## Performance Tips

1. **Hot Reload**: Use `r` in terminal instead of full rebuilds
2. **Selective Compilation**: Use `--split-debug-info` for faster builds
3. **Caching**: The container mounts gradle/android caches automatically
4. **Web Development**: Use `flutter run -d chrome` for fastest iteration

## File Structure

```
.devcontainer/
├── devcontainer.json       # Main container configuration
├── docker-compose.yml      # Docker Compose for Windows/WSL compatibility
├── Dockerfile              # Custom build instructions (optional)
├── post-create.sh          # Setup script (runs after container creation)
├── .env                    # Environment variables
├── README.md               # This file
└── EXTENSIONS.md           # Extensions documentation

.vscode/
├── launch.json             # Debug configurations
├── tasks.json              # Build/run tasks
└── settings.json           # VS Code settings optimized for Flutter
```

## Support & Resources

- [Flutter Documentation](https://flutter.dev/docs)
- [Dev Containers Documentation](https://containers.dev/)
- [Dart Language Guide](https://dart.dev/guides)
- [VS Code Remote Development](https://code.visualstudio.com/docs/remote/remote-overview)
