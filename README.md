<h1 align="center">Head First Practices</h1>
<p align="center">
    <a href="https://github.com/rios0rios0/hf-practices/releases/latest">
        <img src="https://img.shields.io/github/release/rios0rios0/hf-practices.svg?style=for-the-badge&logo=github" alt="Latest Release"/></a>
    <a href="https://github.com/rios0rios0/hf-practices/blob/main/LICENSE">
        <img src="https://img.shields.io/github/license/rios0rios0/hf-practices.svg?style=for-the-badge&logo=github" alt="License"/></a>
</p>

A collection of five iOS practice applications built with Objective-C in Xcode, following the exercises from the "Head First iPhone and iPad Development" book series. Each sub-project explores different UIKit components and iOS SDK features, progressing from basic view management to Core Data persistence and Twitter integration. Development discontinued on 2012-02-16.

## Sub-Projects

### Hello World

A multi-view exploration app with six different `UIViewController` subclasses demonstrating:

- **View switching** with a `SwitchViewController` that manages transitions between views
- **`UIPickerView`** with data loaded from a plist file (`DBPickerView.plist`)
- **Action sheets** with custom button images (normal and selected states)
- **Multiple XIB files** (View2 through View6), each with its own controller
- **Image handling** with landscape and portrait Apple logos

### iDecide

A minimal decision-making app with a single button that displays "Go for it!" when pressed. Demonstrates basic `IBAction` wiring and `UILabel` updates.

### DrinkMixer

A full CRUD drink recipe manager featuring:

- **`UITableView`** with `UINavigationController` for master-detail navigation
- **Add, edit, and delete** drinks with a modal `AddViewController`
- **Plist-based persistence** -- drinks are loaded from and saved to `DrinkDirections.plist`
- **Edit mode** with swipe-to-delete and an Edit button in the navigation bar
- **`NSNotificationCenter`** observer to save data on application termination
- **Constants file** (`DrinkConstats.h`) defining `NAME_KEY` and `DIRECTION_KEY`

### InstaTwit

A Twitter posting app combining:

- **Two-component `UIPickerView`** -- activities ("sleeping", "eating", "working"...) and feelings ("awesome", "sad", "happy"...)
- **`UITextField`** for custom notes
- **Twitter API integration** via `NSMutableURLRequest` with HTTP POST to the Twitter status update XML endpoint
- **Keyboard dismissal** via `DoneKeyBoard:` action

### iBountyHunter

The most advanced sub-project, implementing:

- **Core Data** persistence with a `Fugitive` managed object entity
- **`NSFetchedResultsController`** for efficient table view data sourcing with sort descriptors
- **Tab-based navigation** with separate lists for fugitives and captured targets
- **Detail views** for viewing fugitive information (`FugitiveDetailViewController`)
- **Camera integration** for captured photo documentation (`CapturedPhotoViewController`)
- **Cache-based fetching** with named caches (`captured_list.cache`)

## Technologies

- **Objective-C** (manual reference counting, no ARC)
- **Xcode** with Interface Builder (XIB files)
- **UIKit** -- `UITableView`, `UIPickerView`, `UINavigationController`, `UIActionSheet`, `UIAlertView`
- **Core Data** -- `NSManagedObjectContext`, `NSFetchedResultsController`, `NSEntityDescription`
- **Foundation** -- `NSMutableArray`, `NSDictionary`, plist serialization
- **iOS SDK 5.x** (deployment target era)

## Project Structure

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
│       ├── DrinkConstats.h               # Key constants
│       └── DrinkDirections.plist         # Drink recipe data
├── InstaTwit/
│   ├── InstaTwit.xcodeproj/
│   └── InstaTwit/
│       ├── AppDelegate.{h,m}
│       └── ViewController.{h,m}          # Picker + Twitter posting
├── iBountyHunter/
│   ├── iBountyHunter.xcodeproj/
│   └── iBountyHunter/
│       ├── AppDelegate.{h,m}
│       ├── Fugitive.{h,m}                       # Core Data entity
│       ├── FugitiveListViewController.{h,m}      # Fugitive list with NSFetchedResultsController
│       ├── FugitiveDetailViewController.{h,m}    # Fugitive detail view
│       ├── CapturedListViewController.{h,m}      # Captured fugitives list
│       └── CapturedPhotoViewController.{h,m}     # Camera photo capture
└── LICENSE
```

## Installation

1. Install **Xcode** (the projects were created with Xcode 4.x for iOS SDK 5.x)
2. Open any sub-project's `.xcodeproj` file
3. Select an iOS Simulator target and build (Cmd+B) then run (Cmd+R)

> **Note:** These projects use manual reference counting (MRC). Modern Xcode versions may require disabling ARC or adding `-fno-objc-arc` compiler flags.

## Contributing

Contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

This project is licensed under the terms specified in the [LICENSE](LICENSE) file.
