# Head First Practices Repository

This repository is a collection of five iOS practice applications built with Objective-C in Xcode, following the exercises from the "Head First iPhone and iPad Development" book series. Each sub-project explores different UIKit components and iOS SDK features, progressing from basic view management to Core Data persistence and Twitter integration. Development was discontinued on 2012-02-16 and the repository is preserved as a historical reference.

**ALWAYS** reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.

## Working Effectively

### Bootstrap and Test the Repository

There is no build system, package manager, or test suite. All sub-projects are standalone Xcode projects interpreted by macOS and iOS SDK tools.

To open and build a sub-project:

1. Install **Xcode 4.x** (or the closest available modern version on macOS)
2. Open any sub-project's `.xcodeproj` file:
   ```sh
   open "Hello World/Hello World.xcodeproj"
   open "iDecide/iDecide.xcodeproj"
   open "DrinkMixer/DrinkMixer.xcodeproj"
   open "InstaTwit/InstaTwit.xcodeproj"
   open "iBountyHunter/iBountyHunter.xcodeproj"
   ```
3. Select an iOS Simulator target in Xcode
4. Build: **Cmd+B** (takes 5-30 seconds depending on the sub-project)
5. Run: **Cmd+R** (launches the selected iOS Simulator)

> **Note:** These projects use manual reference counting (MRC, no ARC). Modern Xcode versions may require adding `-fno-objc-arc` to the compiler flags for each target, or disabling ARC migration prompts.

### Validating Source Files

There is no linter or automated checker. Manual code review is the only validation method:

- Review `.h` and `.m` files directly in Xcode or a text editor
- Confirm `#import` statements reference existing headers within the sub-project
- Check that XIB files are present for each `UIViewController` subclass that requires one
- Verify plist files (`DrinkDirections.plist`, `DBPickerView.plist`) are included in the appropriate target's "Copy Bundle Resources" build phase

## Validation

### Manual Testing Steps

- **ALWAYS** build each sub-project in Xcode before submitting changes (`Cmd+B`)
- Run on the iOS Simulator to confirm the UI loads and interactions work (`Cmd+R`)
- **No automated linting, unit tests, or CI/CD exists** in this repository -- all validation is manual
- For `DrinkMixer`: verify add/edit/delete drink operations persist across app restarts (plist file)
- For `iBountyHunter`: verify Core Data entities load correctly and the tab bar navigation works

### Expected Timing

- **Xcode build (Cmd+B)**: 5-30 seconds per sub-project
- **Simulator launch**: 15-60 seconds (first launch may be slower)
- **App interaction test**: manual, typically <2 minutes per sub-project

## Repository Structure

```
hf-practices/
├── Hello World/
│   ├── Hello World.xcodeproj/
│   └── Hello World/
│       ├── AppDelegate.{h,m}
│       ├── SwitchView.{h,m}              # Custom view
│       ├── SwitchViewController.{h,m}    # Multi-view switcher
│       ├── ViewController{2..6}.{h,m}    # Individual view controllers
│       ├── MainWindow.xib
│       ├── View{2..6}.xib                # View XIBs
│       └── DBPickerView.plist            # Picker data source
├── iDecide/
│   ├── iDecide.xcodeproj/
│   └── iDecide/
│       ├── AppDelegate.{h,m}
│       └── ViewController.{h,m}          # Single-button decision maker
├── DrinkMixer/
│   ├── DrinkMixer.xcodeproj/
│   └── DrinkMixer/
│       ├── AppDelegate.{h,m}
│       ├── MasterViewController.{h,m}    # Table view with drink list
│       ├── DetailViewController.{h,m}    # Drink detail display
│       ├── AddViewController.{h,m}       # Add/edit drink form
│       ├── DrinkConstats.h               # Key constants (NAME_KEY, INGREDIENTS_KEY, DIRECTIONS_KEY)
│       └── DrinkDirections.plist         # Drink recipe data (plist persistence)
├── InstaTwit/
│   ├── InstaTwit.xcodeproj/
│   └── InstaTwit/
│       ├── AppDelegate.{h,m}
│       └── ViewController.{h,m}          # Picker + Twitter posting
├── iBountyHunter/
│   ├── iBountyHunter.xcodeproj/
│   └── iBountyHunter/
│       ├── AppDelegate.{h,m}
│       ├── Fugitive.{h,m}                       # Core Data managed object
│       ├── FugitiveListViewController.{h,m}      # Fugitive list (NSFetchedResultsController)
│       ├── FugitiveDetailViewController.{h,m}    # Fugitive detail view
│       ├── CapturedListViewController.{h,m}      # Captured fugitives list
│       └── CapturedPhotoViewController.{h,m}     # Camera photo capture
├── .github/
│   ├── copilot-instructions.md           # This file
│   └── workflows/
│       └── release.yaml                  # Tag creation on merge to main
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

## Technology Stack

| Component | Details |
|-----------|---------|
| **Language** | Objective-C (manual reference counting, no ARC) |
| **Platform** | iOS (iPhone), iOS SDK 5.x |
| **IDE** | Xcode 4.x with Interface Builder (XIB files) |
| **UI Framework** | UIKit -- `UITableView`, `UIPickerView`, `UINavigationController`, `UIActionSheet`, `UIAlertView` |
| **Persistence** | Plist serialization (`NSMutableArray`, `NSDictionary`) and Core Data (`NSManagedObjectContext`, `NSFetchedResultsController`) |
| **Networking** | `NSMutableURLRequest` with HTTP POST (Twitter API, InstaTwit) |
| **Frameworks** | Foundation, UIKit, Core Data |
| **Dependencies** | None -- all sub-projects are self-contained |

## Architecture and Design Patterns

### Common Patterns Across Sub-Projects

- **MVC** (Model-View-Controller): each sub-project separates view files (XIBs), controller classes (`.h`/`.m`), and data (plist or Core Data)
- **XIB-based UI**: every `UIViewController` has a corresponding `.xib` file managed by Interface Builder; no programmatic layout
- **Manual memory management**: `retain`/`release`/`autorelease` calls throughout; no `@autoreleasepool` blocks or ARC syntax
- **AppDelegate as entry point**: `application:didFinishLaunchingWithOptions:` sets up the root view controller in every sub-project

### Sub-Project Patterns

- **Hello World**: `SwitchViewController` owns and swaps child `UIViewController` instances; each child has its own XIB and is created lazily on first display
- **iDecide**: single `ViewController` with one `IBAction`; no model layer
- **DrinkMixer**: `MasterViewController` (table view) owns an `NSMutableArray` of `NSDictionary` entries; `AddViewController` is presented modally and dismisses with a delegate callback; data persisted to a plist via `NSNotificationCenter` on app termination
- **InstaTwit**: two-component `UIPickerView` backed by `NSArray` literals; network request sent synchronously on main thread (historical anti-pattern)
- **iBountyHunter**: Core Data stack initialized in `AppDelegate`; `NSFetchedResultsController` with named cache drives both tab view controllers; camera integration via `UIImagePickerController`

## Coding Conventions

### Objective-C Style

```objc
// Header: declare properties and IBOutlets
@interface DrinkViewController : UIViewController
@property (nonatomic, retain) NSString *drinkName;
@property (nonatomic, retain) IBOutlet UILabel *nameLabel;
- (IBAction)saveDrink:(id)sender;
@end

// Implementation: synthesize properties, release in dealloc
@implementation DrinkViewController
@synthesize drinkName, nameLabel;

- (void)dealloc {
    [drinkName release];
    [nameLabel release];
    [super dealloc];
}
@end
```

### Key Patterns

- Use `@property (nonatomic, retain)` for object properties; `(nonatomic, assign)` for primitives and delegates
- Always pair `retain` with `release` in `dealloc`
- Use `IBOutlet` for view references and `IBAction` for event handlers wired in Interface Builder
- Plist persistence: load with `[NSArray arrayWithContentsOfFile:]`; save with `[array writeToFile:atomically:YES]`
- Core Data: obtain `NSManagedObjectContext` from `AppDelegate`; create entities with `[NSEntityDescription insertNewObjectForEntityForName:inManagedObjectContext:]`
- Constants declared in header files as `#define` macros (e.g., `DrinkConstats.h` defines `NAME_KEY`, `INGREDIENTS_KEY`, `DIRECTIONS_KEY`)

### File Organization

- Each sub-project is entirely self-contained in its own directory with its own `.xcodeproj`
- Header files (`.h`) declare the public interface; implementation files (`.m`) contain all logic
- XIB files are named after their corresponding view controller class

## CI/CD Pipeline

- **Release workflow** (`.github/workflows/release.yaml`): on push to `main`, delegates to the shared `rios0rios0/pipelines` release workflow to create Git tags. No build or test jobs — all Xcode validation remains manual on macOS.

## Development Workflow

1. Fork and clone the repository
2. Create a branch: `git checkout -b feat/my-change`
3. Open the relevant sub-project `.xcodeproj` in Xcode
4. Make changes to `.h`, `.m`, `.xib`, or plist files
5. Build (`Cmd+B`) and fix any compiler errors or warnings
6. Run on the iOS Simulator (`Cmd+R`) and manually verify the expected behaviour
7. Commit following the [commit conventions](https://github.com/rios0rios0/guide/wiki/Life-Cycle/Git-Flow)
8. Open a pull request against `main`

## Common Tasks

### Opening a Sub-Project

```sh
# From the repository root
open "Hello World/Hello World.xcodeproj"
open "DrinkMixer/DrinkMixer.xcodeproj"
open "iBountyHunter/iBountyHunter.xcodeproj"
```

### Adding a New View Controller (Objective-C)

1. In Xcode, select the sub-project target and add a new `UIViewController` subclass (File → New → File → Cocoa Touch Class)
2. Check "Also create XIB file" to generate the corresponding `.xib`
3. Wire `IBOutlet` and `IBAction` connections in Interface Builder
4. Present or push the new controller from an existing controller

### Modifying Plist Data (DrinkMixer)

1. Open `DrinkDirections.plist` in Xcode's property list editor or a text editor
2. Each entry is a dictionary with keys `name`, `ingredients`, and `directions` (defined in `DrinkConstats.h`)
3. Build and run to verify data loads correctly in `MasterViewController`

### Working with Core Data (iBountyHunter)

1. Open the `.xcdatamodeld` file in Xcode's data model editor to inspect or modify the `Fugitive` entity
2. If the entity schema changes, delete the app from the Simulator (to drop the existing SQLite store) before re-running
3. Fetch requests and sort descriptors are configured in `FugitiveListViewController` and `CapturedListViewController`

## Troubleshooting

- **"ARC is enabled" migration prompt**: dismiss the prompt and add `-fno-objc-arc` to Build Settings → Other C Flags for each target, or set "Objective-C Automatic Reference Counting" to `NO`
- **XIB file not found at runtime**: confirm the XIB is listed in the target's "Copy Bundle Resources" build phase
- **Plist not saving**: check that the file path is constructed using `NSSearchPathForDirectoriesInDomains` pointing to the Documents directory, not the bundle (which is read-only)
- **Core Data crash on schema change**: delete the app from the Simulator to remove the stale `.sqlite` store, then re-run
- **Twitter POST fails (InstaTwit)**: the Twitter v1 API used by this app was retired in 2013; network requests will fail with a 410 or authentication error -- this is expected

## Prerequisites

- **macOS** with [Xcode](https://developer.apple.com/xcode/) installed (Xcode 4.x era; modern Xcode requires MRC flag adjustments)
- An **iOS Simulator** target configured in Xcode (iPhone 4/4S running iOS 5.x for best compatibility)
- No external dependencies, package managers, or build tools are required
