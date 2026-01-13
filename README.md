# Pawfinder

Pawfinder is a Flutter app that helps FRC scouts quickly record match details, manage schedules, and share strategy notes. The UI is scaffolded but intentionally lightweight so teams can fill in stage-specific forms and data sync flows.

## What’s here

- Flutter app shell wired with `go_router` navigation and a drawer-based `NavBar` for quick access to Schedule, Match (with Auto/Tele/End sub-stages), Strat, and Pits pages.
- `flutter_riverpod` ready for shared state; current user selection is stored globally (`currentUser`) from the drawer user list.
- Placeholder pages for match stages (`AutoPage`, `TelePage`, `EndPage`) so you can drop in scoring inputs.
- Material 3 theming with a seed color; easy to retheme.

## Prerequisites

- Flutter SDK (stable) with Dart 3.9+ (`flutter --version` should show Dart >= 3.9).
- Xcode for iOS builds, Android SDK/Android Studio for Android, Chrome for web.
- Device or emulator configured for your target platform.

## Getting started

1) Install Flutter: <https://docs.flutter.dev/get-started/install>
2) Fetch deps:

 ```bash
 flutter pub get
 ```

1) Run the app on your preferred device (examples):

 ```bash
 # Chrome (web)
 flutter run -d chrome

 # Android emulator or device
 flutter run -d android

 # iOS simulator
 flutter run -d ios
 ```

1) Use hot reload while iterating: press `r` in the running terminal or your IDE’s reload button.

## Project layout

- `lib/main.dart` – app entrypoint, `GoRouter` setup, and Material theme.
- `lib/custom_widgets/nav_bar.dart` – drawer-based navigation and user picker.
- `lib/pages/` – top-level pages: Schedule, Match, Strat, User; match stage stubs in `match_stages/`.
- `pubspec.yaml` – dependencies (`flutter_riverpod`, `go_router`, `loading_animation_widget`, etc.).

## Development notes

- Navigation: drawer items call `router.go(...)`; add new routes to `MyApp.router` in `main.dart`.
- State: Riverpod is available—create providers for shared scouting data instead of globals once forms are built.
- Users: update the `users` list in `NavBar` to add team members; selection sets `currentUser` and routes to User page.
- The Match stage pages currently throw `UnimplementedError`; add your form widgets there.

## Testing & linting

- Run tests: `flutter test`
- Follow `flutter_lints` defaults; format with `dart format .` if needed.

## Contributing

Feel free to open PRs for:

- Adding scoring inputs to Auto/Tele/End pages.
- Hooking up data storage (local or cloud) and sync for offline-first scouting.
- Improving schedule import, strategy board, and pits view once routes are defined.
