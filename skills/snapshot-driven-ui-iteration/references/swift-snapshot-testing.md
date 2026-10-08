# Swift snapshot setup

Use this reference when a SwiftUI or UIKit project needs a rendering fixture. Reuse the project's existing test framework and snapshot conventions first.

## Dependency and test target

Use [swift-snapshot-testing](https://github.com/pointfreeco/swift-snapshot-testing) if the project has no suitable renderer. Select a version compatible with the project's Swift toolchain and platform requirements. Check the API in that version before using the example below.

Add the package through the project's existing dependency setup. Link its product to the test target that contains the fixture:

```swift
.product(name: "SnapshotTesting", package: "swift-snapshot-testing")
```

Record whether this setup is temporary or part of permanent test coverage.

## SwiftUI example

This example uses Swift Testing. Replace `FeatureModule`, `FeatureView`, and the dimensions with the actual module, view setup, and target viewport. The dimensions are examples, not defaults.

```swift
import Testing
import SnapshotTesting
import SwiftUI
@testable import FeatureModule

@MainActor
struct FeatureViewSnapshotTests {
    @Test
    func test_default_state_snapshot() {
        // Given: supply stable data and required environment values.
        let view = FeatureView()

        // When / Then: render an image for visual review.
        withSnapshotTesting(record: .all) {
            assertSnapshot(
                of: view,
                as: .image(layout: .fixed(width: 390, height: 844))
            )
        }
    }
}
```

Use device layout or trait configuration when safe areas, size classes, or display scale affect the result. A fixed container does not reproduce all device behavior. Set traits to match the target state, rather than forcing an iPad layout.

For UIKit, render the actual view or view controller with the library's image strategy. Follow the same state and viewport checks.

## Run and inspect

Discover the workspace or project, scheme, test target, and available simulator. Run only the fixture with the selected destination:

```sh
xcodebuild test \
  -workspace '<workspace>.xcworkspace' \
  -scheme '<scheme>' \
  -destination 'platform=iOS Simulator,id=<simulator-udid>' \
  -only-testing:'<test-target>/<snapshot-suite>'
```

Use `-project` instead of `-workspace` when the project requires it. Use the appropriate destination for other Apple platforms. Preserve the full run log for diagnosis.

The library reports recorded image paths. Use those paths instead of assuming a fixed directory. In record mode, it can report a test failure after writing an image. Confirm that the image exists and inspect it. Other failures still require diagnosis.

For permanent regression tests, review the baseline, remove forced recording, and run comparison mode in the same rendering environment. Keep simulator, OS, and display settings consistent to limit image differences caused by the environment.

API and recording behavior: [library usage guide](https://github.com/pointfreeco/swift-snapshot-testing#usage). SwiftUI layout options: [implementation](https://github.com/pointfreeco/swift-snapshot-testing/blob/main/Sources/SnapshotTesting/Snapshotting/SwiftUIView.swift).
