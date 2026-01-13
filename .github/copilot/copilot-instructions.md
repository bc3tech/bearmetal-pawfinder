# GitHub Copilot Instructions for Paw-Finder

This document guides GitHub Copilot to generate code consistent with the Paw-Finder project's technology stack, architecture, and established patterns.

## Priority Guidelines

When generating code for this repository, follow these guidelines in order:

1. **Version Compatibility**: Strictly respect the Dart SDK version (^3.9.0) and Flutter framework versions specified in pubspec.yaml
2. **Technology Stack**: Generate code using the established dependencies (Flutter Riverpod, Go Router, HTTP client, UUID generation)
3. **Codebase Patterns**: Match the patterns and conventions found in existing code before applying external best practices
4. **Architectural Consistency**: Maintain the MaterialApp routing structure with nested ShellRoutes and ConsumerStatefulWidget patterns
5. **Code Quality**: Prioritize maintainability, readability, and consistency with existing Flutter/Dart conventions

## Technology Versions

### Dart SDK
- **Version**: ^3.9.0 (minimum Dart 3.9.0)
- **Language Features**: Use modern Dart 3.x syntax including records, extensions, and pattern matching where appropriate
- **Null Safety**: All code must be null-safe; no legacy Dart code

### Flutter Framework
- **Version**: Latest stable (specified by Flutter SDK)
- **Architecture Pattern**: Material Design (uses MaterialApp)
- **Widget Type Preference**: Stateful and StatelessWidget components

### Key Dependencies
- **flutter_riverpod**: ^2.1.5 - State management solution
- **go_router**: ^17.0.0 - Navigation and routing
- **path_provider**: ^2.1.5 - File system access
- **http**: ^1.6.0 - HTTP client for API calls
- **uuid**: ^4.5.1 - UUID generation
- **loading_animation_widget**: ^1.3.0 - UI animations
- **cupertino_icons**: ^1.0.8 - iOS-style icons
- **meta**: ^1.16.0 - Metadata annotations

### Linting Configuration
- **Base**: flutter_lints/flutter.yaml (flutter_lints: ^5.0.0)
- **Analysis**: All code must pass `flutter analyze` with no warnings
- **Formatting**: Code must follow `dart format` standards (two-space indentation)

## Project Architecture

### Folder Structure
```
lib/
├── main.dart              # Application entry point and routing configuration
├── custom_widgets/        # Reusable custom widget components
│   ├── nav_bar.dart      # Navigation drawer and app bar scaffold
│   └── numerical_button.dart
└── pages/                 # Full-page screen widgets
    ├── match.dart
    ├── schedule.dart
    ├── strat.dart
    ├── user.dart
    └── match_stages/      # Sub-routes for match scouting phases
```

### Routing Architecture
- **Primary Router**: GoRouter configured in MyApp class (main.dart)
- **Nested Navigation**: Uses nested ShellRoutes to maintain NavBar context across route changes
- **Route Paths**: Follow the pattern `/PageName` or `/Match/StageName`
- **Navigation Method**: Use `GoRouter.go()` for navigation (see NavBar implementation)

### State Management
- **Framework**: Flutter Riverpod 2.1.5
- **Widget Pattern**: ConsumerStatefulWidget for widgets that need state management
- **Widget Extension Pattern**: Create ConsumerState subclasses with _State naming suffix

## Code Patterns and Conventions

### Widget Declaration
All widget classes follow this exact pattern:
```dart
// Use ConsumerStatefulWidget for state management integration
class MyWidget extends ConsumerStatefulWidget {
  const MyWidget({super.key});

  @override
  ConsumerState<ConsumerStatefulWidget> createState() {
    return MyWidgetState();
  }
}

class MyWidgetState extends ConsumerState<MyWidget> {
  @override
  Widget build(BuildContext context) {
    // Implementation
  }
}
```

### StatelessWidget Pattern
For simple widgets without state management:
```dart
class SimpleWidget extends StatelessWidget {
  const SimpleWidget({super.key});

  @override
  Widget build(BuildContext context) {
    // Implementation
  }
}
```

### Imports Organization
Follow this import order:
1. Dart SDK imports (`dart:...`)
2. Flutter imports (`package:flutter/...`)
3. Internal package imports (`package:beariscope_scouter/...`)

Example from existing code:
```dart
import 'package:flutter/material.dart';
import 'package:flutter/src/widgets/framework.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:go_router/go_router.dart';
import 'package:loading_animation_widget/loading_animation_widget.dart';

import '../pages/user.dart';
```

### Class and Method Naming
- **Class Names**: PascalCase (e.g., `NavBar`, `UserPage`, `SchedulePage`)
- **State Classes**: `<WidgetName>State` (e.g., `NavBarState` for NavBar)
- **Constants**: camelCase for variables, PascalCase for class constants
- **Private Members**: Prefix with underscore (e.g., `_itemCount`)

### Navigation Implementation
Always use GoRouter for navigation:
```dart
// Correct - using GoRouter.go()
widget.router.go('/User');

// DO NOT use Navigator.of(context).push() or other legacy navigation
```

### Widget Properties
- Use required named parameters for all widget constructors
- Always include `super.key` in const constructors
- Document property purposes where non-obvious

Example from NavBar:
```dart
const NavBar({
  super.key,
  required this.page,
  required this.title,
  required this.router,
  this.devMode = false,
});
```

## Code Quality Standards

### Maintainability
- Write self-documenting code with clear variable and method names
- Keep widgets focused on single responsibilities
- Extract complex UI into separate custom widgets
- Use consistent spacing and indentation (2 spaces per level)
- Limit widget build methods to reasonable complexity

### Documentation
- Add comments for non-obvious logic
- Document widget purposes at the class level if complex
- Use inline comments sparingly; let code be self-documenting
- Document API integration points and data models

Example style from existing code:
```dart
// Minimal but purposeful comments
class NavBar extends StatefulWidget {
  final Widget page;
  final String title;
  final GoRouter router;
  final bool devMode;

  const NavBar({
    super.key,
    required this.page,
    required this.title,
    required this.router,
    this.devMode = false,
  });
```

### Error Handling
- Use try/catch for HTTP requests and file operations
- Provide meaningful error messages to users
- Log errors using print() for debugging (avoid logging framework)
- Gracefully handle null values with null-aware operators

### Performance
- Use const constructors for stateless widgets and constants
- Use ListView.builder for large lists (pattern not yet present, follow best practices)
- Avoid rebuilding entire widgets; use ConsumerWidget selectively
- Use const constructors throughout to minimize rebuild overhead

## Testing

### Test Structure
- Test files located in `test/` directory
- Use `flutter_test` package
- Follow naming convention: `<feature>_test.dart`

Current test pattern (from widget_test.dart):
```dart
import 'package:flutter_test/flutter_test.dart';

void main() {
  test('temporary always passes', () {
    expect(true, isTrue);
  });
}
```

### Testing Guidelines
- Write widget tests for custom widgets
- Test user interactions and navigation
- Use `WidgetTester` for UI testing
- Mock external dependencies (HTTP calls, etc.)
- Aim for meaningful test coverage of critical paths

## Dart Language Guidelines

### Modern Dart 3.x Features
- Use `final` instead of `var` where type is not obvious
- Use `const` for immutable values
- Use `required` for mandatory named parameters
- Use `late` for lazy initialization
- Leverage null-safety with `?` and `late` keywords

### Code Style
- Two-space indentation (enforced by dart format)
- Private fields prefixed with underscore
- Use extension methods for readability
- Avoid mutable global state (see currentUser in user.dart as exception, not pattern)

### Collections
- Use List, Set, Map from Dart core library
- Use spread operator (...) for list/map construction
- Use forEach only in limited contexts; prefer for-in loops

## Flutter-Specific Guidelines

### Widget Lifecycle
- Use `@override` annotation for all overridden methods
- Initialize state in `initState()` when needed
- Clean up resources in `dispose()`
- Use `didChangeDependencies()` rarely and with clear purpose

### Material Design
- Use Material Design 3 widgets where available
- Apply consistent colors from the theme (currently seedColor: Colors.deepPurple)
- Use standard Material widgets (Scaffold, AppBar, Drawer, etc.)
- Maintain accessibility standards

### Riverpod Integration
- Create providers for shared state
- Use ConsumerWidget or ConsumerStatefulWidget for state access
- Follow provider naming convention: `<name>Provider`
- Document provider purposes

## Version Control and Code Organization

### File Organization
- One public widget class per file (with associated State class)
- Support classes and models can be in the same file
- Use meaningful file names that reflect content
- Keep file sizes reasonable (typically < 400 lines)

### Comments and Commits
- Use descriptive commit messages
- Reference issue numbers when applicable
- Keep commits focused and atomic

## General Best Practices for This Project

1. **Consistency First**: When in doubt about a pattern, scan similar files and match their approach
2. **Widget Reusability**: Extract reusable UI into custom widgets in the `custom_widgets/` folder
3. **Navigation**: Always use GoRouter; never use legacy Navigator API
4. **State Management**: Use Riverpod ConsumerStatefulWidget for state; avoid setState() complexity
5. **Testing**: Add tests for new features; maintain test compatibility with pubspec.yaml
6. **Performance**: Prioritize const constructors and efficient rebuilds
7. **Formatting**: Run `dart format .` before committing
8. **Analysis**: Run `flutter analyze` and fix all warnings

## Project-Specific Guidance

### Application Name
- Project name: `beariscope_scouter`
- User-facing name: "Paw-Finder"
- Use "Paw Finder" in UI text and comments

### Current Build Configuration
- Primarily targets Material Design
- Multi-platform support (Android, iOS, Web, Linux, macOS, Windows)
- Uses Material-based UI components exclusively

### Key Application Routes
- `/` - Schedule page (default route)
- `/Match` - Match scouting view
- `/Match/Auto` - Autonomous period scouting
- `/Match/Tele` - Teleoperated period scouting
- `/Match/End` - End game scouting
- `/Strat` - Strategy page
- `/User` - User profile page
- `/Pits` - Pit scouting (route exists but page not implemented)

### Important: Scan Before Generating
Always examine related existing code in the codebase before generating new code. Match the established patterns for:
- Widget structure and naming
- Import organization
- Navigation implementation
- State management approach
- UI layout and Material Design patterns
- Documentation style
