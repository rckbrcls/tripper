# Contributing to Tripper

Tripper is a SwiftUI iOS app prototype. Contributions should keep the repository aligned with the current app structure, avoid generic scaffolding, and document the difference between implemented behavior and planned product scope.

## Project Entry Point

Use the Xcode workspace:

```sh
cd tripper
open tripper.xcworkspace
```

Do not open `tripper.xcodeproj` directly for normal development. The project uses CocoaPods, and the workspace is the correct integration point for app, pod, and package dependencies.

## Development Guidelines

- Keep app strings, Swift comments, TODOs, examples, and documentation in English.
- Keep Swift code in English.
- Group SwiftUI work by the existing feature folders under `tripper/tripper/Views/`.
- Prefer small, focused SwiftUI components when a screen starts mixing unrelated responsibilities.
- Keep reusable visual modifiers in `tripper/tripper/Styles/`.
- Keep shared services in `tripper/tripper/Services/` only when they represent app behavior, not screen-only state.
- Do not add a backend, API layer, database layer, or authentication abstraction unless the implementation actually introduces those capabilities.
- Do not document planned requirements as implemented features.

## Dependencies

Tripper currently uses CocoaPods and Swift Package Manager:

- CocoaPods dependencies are declared in `tripper/Podfile`.
- Locked pod versions are stored in `tripper/Podfile.lock`.
- SwiftPM resolution is stored in `tripper/tripper.xcworkspace/xcshareddata/swiftpm/Package.resolved`.

Rules for dependency changes:

- Do not edit files under `tripper/Pods/` manually.
- Add or remove pods through `Podfile`, then regenerate the pod workspace with CocoaPods.
- Keep dependency additions justified by current app needs.
- Update `README.md` when setup or dependency expectations change.

## Testing and Verification

Use Xcode for verification:

- Build with `Product > Build`.
- Run tests with `Product > Test`.
- Use the shared `tripper` scheme.

The current XCTest and XCUITest files are mostly generated placeholders. When adding real behavior, prefer tests around services, data transformations, and stable view logic instead of only launch tests.

Before submitting a change, check:

- The app still opens from `tripper/tripper.xcworkspace`.
- User-visible copy is in English.
- Documentation matches the actual code.
- No user-specific Xcode state is intentionally added.
- No `.DS_Store` files are intentionally added or modified.
- Generated or vendored files are only changed when the dependency workflow requires it.

## Documentation Rules

Update documentation when a change affects:

- Implemented user flows.
- Setup requirements.
- Dependency management.
- Test workflow.
- Privacy, permissions, authentication, or persistence behavior.
- The relationship between `REQUIREMENTS.md` and implemented app behavior.

Keep documentation specific to Tripper. Avoid broad templates that could apply to any iOS app.

## Pull Request Expectations

A good pull request should include:

- A clear description of what changed.
- The user flow or source area affected.
- Notes about verification performed.
- Screenshots or screen recordings for visible UI changes when practical.
- Documentation updates when behavior, setup, or scope changes.

If a change implements a planned requirement, update the implementation-alignment section in `REQUIREMENTS.md`.

## Code of Conduct

Participation in this project is governed by [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).
