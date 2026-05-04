# Tripper

Tripper is an iOS travel-planning app prototype for building road-trip itineraries with destination search, route preferences, map-based trip previews, saved-route concepts, and profile settings.

The repository currently contains one SwiftUI iOS app. It is not a backend, API service, web app, desktop app, or monorepo. The product requirements are broader than the implementation; treat the current codebase as an early mobile prototype, not as a production-ready travel platform.

## Current Status

The implemented app is a visual and interaction prototype with partial MapKit behavior:

- A splash screen and four-tab SwiftUI shell: Explore, Trip, Saved, and Profile.
- Explore screens for route search inputs, filters, and preference controls.
- Trip screens with mock map points, trip summary UI, map transitions, point list interactions, and route continuation views.
- MapKit/CoreLocation integration for current location, local search, reverse geocoding, Look Around previews, and route overlays.
- Saved routes and profile/settings screens backed by placeholder or local UI state.
- Local `@AppStorage` state for login status and theme selection.
- Generated XCTest and XCUITest targets with placeholder tests.

Not implemented yet:

- Real trip persistence.
- Backend API or external application server.
- Real authentication or social login.
- Reviews, ratings, traffic alerts, trip sharing, or recommendation data services.
- Production deployment, release automation, or CI/CD.

## Product Scope

The product concept is documented in [REQUIREMENTS.md](REQUIREMENTS.md). That file describes the intended travel-planning system, user stories, MVP direction, actors, and A/B testing ideas.

Use the requirements document as product direction, but verify implementation details in the Swift code. Several requirements are planned and not currently wired into the app.

## Technology Stack

| Area | Current technology |
| --- | --- |
| Platform | iOS |
| UI | SwiftUI |
| Maps and location | MapKit, CoreLocation |
| Local state | SwiftUI state and `@AppStorage` |
| Project system | Xcode project and workspace |
| Dependency management | CocoaPods and Swift Package Manager |
| CocoaPods | `ScalingHeaderScrollView`, `ProgressIndicatorView`, `ExytePopupView`, `FloatingButton` |
| SwiftPM | `FluidGradient` |
| Tests | XCTest and XCUITest generated targets |
| License | GPLv3, see [LICENSE](LICENSE) |

The app target is configured with Swift 5.0 and an iOS deployment target of 18.0 in `tripper/tripper.xcodeproj/project.pbxproj`. Test targets are configured separately.

## Repository Structure

```text
.
├── README.md                    # Technical onboarding and current implementation overview
├── REQUIREMENTS.md              # Product requirements, planned scope, and implementation alignment
├── CONTRIBUTING.md              # Tripper-specific contribution workflow
├── CODE_OF_CONDUCT.md           # Community behavior expectations
├── LICENSE                      # GPLv3 license text
└── tripper/
    ├── Podfile                  # CocoaPods dependency declaration
    ├── Podfile.lock             # Locked pod versions
    ├── Pods/                    # Checked-in CocoaPods vendor state
    ├── tripper.xcworkspace      # Main Xcode entrypoint
    ├── tripper.xcodeproj        # Xcode project
    ├── tripper/                 # App source
    ├── tripperTests/            # XCTest target
    └── tripperUITests/          # XCUITest target
```

## App Source Map

```text
tripper/tripper/
├── tripperApp.swift             # SwiftUI app entrypoint and global environment objects
├── Views/
│   ├── Main/                    # Splash, shell, tab bar, and tab content switching
│   ├── Explore/                 # Explore landing, search, filters, and route preference UI
│   ├── Trip/                    # Trip prototype, map preview, point list, and full-map trip flow
│   ├── Map/                     # Map sheet and floating map controls
│   ├── Saved/                   # Saved-route collection mock UI
│   ├── Profile/                 # Profile, settings, appearance, account, and support screens
│   ├── Credential/              # Placeholder login and signup stepper
│   └── Components/              # Shared SwiftUI controls
├── Services/
│   └── LocationService.swift    # MapKit search, CoreLocation updates, and reverse geocoding
├── Utils/
│   ├── ColorSchemeManager.swift # Theme observation helper
│   └── LocationManager.swift    # Alternate CoreLocation wrapper, currently separate from LocationService
├── Styles/                      # Reusable SwiftUI view modifiers
├── Assets/                      # App icons, logos, medals, map/travel imagery, and social icons
├── CoreData/                    # Default Core Data model file; no active persistence stack identified
└── MapView.swift                # Full-screen map view with search results, Look Around, and route overlay
```

## Runtime Requirements

- macOS with Xcode installed.
- An iOS simulator or physical iOS device supported by the configured deployment target.
- CocoaPods if dependencies need to be reinstalled or regenerated.
- Network access for Apple MapKit services when testing map search, route calculation, reverse geocoding, and Look Around.
- Location permission in the simulator or device for current-location behavior.

No `.env` file, API key file, database setup, server process, or local backend was identified in the current repository.

## Setup

The Xcode workspace is the correct entrypoint because the project uses CocoaPods.

1. Open a terminal in the repository root.
2. Move into the iOS project folder:

   ```sh
   cd tripper
   ```

3. If the CocoaPods sandbox is missing or out of sync, install pods from the same folder:

   ```sh
   pod install
   ```

4. Open the workspace, not the project:

   ```sh
   open tripper.xcworkspace
   ```

5. In Xcode, select the `tripper` scheme and an iOS simulator or device.

The repository currently includes `Pods/`, but dependencies should still be managed through `Podfile` and `Podfile.lock`. Do not edit files inside `tripper/Pods/` manually.

## Build, Run, and Test Workflow

There are no package scripts, Makefile targets, or custom CLI commands in this repository.

Use Xcode for app lifecycle tasks:

- Build: `Product > Build`
- Run in simulator/device: `Product > Run`
- Run tests: `Product > Test`
- Archive: `Product > Archive`

The shared scheme at `tripper/tripper.xcodeproj/xcshareddata/xcschemes/tripper.xcscheme` includes both the unit test target and the UI test target.

The existing tests are generated placeholders:

- `tripper/tripperTests/tripperTests.swift`
- `tripper/tripperUITests/tripperUITests.swift`
- `tripper/tripperUITests/tripperUITestsLaunchTests.swift`

Add meaningful tests when behavior moves out of placeholder UI and into testable services, view models, or data logic.

## Current User Flows

### Explore

`Views/Explore/ExploreView.swift` shows the Tripper logo, current-location button, search entry point, route filters, and placeholder cards. `Views/Explore/Templates/SearchTripView.swift` contains a route search form with origin, destination, distance, duration, and action buttons.

### Trip

`Views/Trip/TripView.swift` holds mock `MapPoint` data, a map preview, trip progress, trip summary, editable point list interactions, and a "Continue trip" action. `Views/Trip/MapWithPoints.swift` expands the trip into a full-screen map-style view with horizontal point cards.

### Map

`MapView.swift` renders a MapKit map, selected search results, current location, route polyline, and optional Look Around preview. It uses `LocationService` for location and search-related behavior.

### Saved Routes

`Views/Saved/SavedRoutes.swift` displays a grid of repeated mock saved route collections using bundled Andes images. It does not persist user-created routes.

### Profile and Settings

`Views/Profile/ProfileView.swift` switches between profile and settings content. Settings screens cover appearance, account information, password, address, notifications, support, and logout UI. Theme selection is stored locally in `@AppStorage("selectedTheme")`.

### Login and Signup

`Views/Credential/Login/LoginView.swift` and `Views/Credential/Signin/StepperView.swift` are placeholder flows. Login state is represented by `@AppStorage("isLoggedIn")`; there is no real identity provider, password validation, token storage, or backend session.

## Data and Persistence

The current implementation uses local SwiftUI state and `@AppStorage` for small UI preferences:

- `isLoggedIn`
- `selectedTheme`

A Core Data model exists at `tripper/tripper/CoreData/tripper.xcdatamodeld`, but no active Core Data stack or trip persistence flow was identified. The model currently contains a default `Item` entity with an optional `timestamp`.

## External Services and Permissions

The app uses Apple platform services through MapKit and CoreLocation:

- Location authorization is requested by `LocationService`.
- Current coordinates are used for map behavior and reverse geocoding.
- `MKLocalSearchCompleter`, `MKLocalSearch`, and `MKDirections` are used for place search and route calculation.
- `MKLookAroundSceneRequest` is used for Look Around previews.

The Xcode project defines `NSLocationWhenInUseUsageDescription` through generated Info.plist build settings.

## Security and Privacy Notes

- The app requests location access and should explain that clearly to users.
- Authentication is a prototype-only local flag. Do not treat it as secure user authentication.
- Google and Facebook buttons are visual placeholders; no OAuth flow was identified.
- Password fields exist in the UI, but credentials are not sent to a backend or stored in Keychain.
- No secrets, API keys, backend tokens, or private configuration files were identified.
- If real accounts, trips, recommendations, reviews, or sharing are added later, the project will need a proper authentication model, secure storage strategy, data retention policy, and privacy review.

## Troubleshooting

| Symptom | Likely cause | Resolution |
| --- | --- | --- |
| Xcode cannot find pod frameworks | The project was opened through `tripper.xcodeproj` or pods are out of sync | Open `tripper/tripper.xcworkspace`; run `pod install` from `tripper/` if needed |
| CocoaPods reports sandbox mismatch | `Podfile.lock` and `Pods/Manifest.lock` differ | Run `pod install` from `tripper/` |
| `FluidGradient` is unavailable | SwiftPM packages have not resolved | Let Xcode resolve packages from the workspace |
| Current city stays as `Loading...` | Location permission is denied or simulator location is not configured | Grant location permission and configure a simulator location |
| Login accepts empty credentials | Login is placeholder-only | Implement real authentication before relying on login state |
| Saved routes do not reflect user-created trips | Saved routes are mock UI | Add persistence before treating this as real saved-trip behavior |

## Documentation Policy

- Keep repository documentation in English.
- Keep app strings, Swift comments, TODOs, examples, and generated docs in English unless a future product requirement explicitly says otherwise.
- Update [REQUIREMENTS.md](REQUIREMENTS.md) when planned product scope changes.
- Update this README when implemented behavior, setup, dependencies, or project structure changes.

## License

This repository includes the GNU General Public License v3.0 text in [LICENSE](LICENSE).
