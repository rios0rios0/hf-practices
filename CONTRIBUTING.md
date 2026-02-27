# Contributing

> **This project was discontinued in February 2012 and is no longer actively maintained.**
> The repository is preserved as a historical reference. No new features or bug fixes are planned.

## Historical Build Information

This project was built using the following tools and technologies:

- **Language:** Objective-C (manual reference counting / MRC)
- **Platform:** iOS (iPhone), iOS SDK 5.x
- **Frameworks:** UIKit, Foundation, Core Data (`NSManagedObjectContext`, `NSFetchedResultsController`), Twitter API
- **IDE:** Xcode 4.x with Interface Builder (XIB files)

### Build Steps (Historical)

The repository contains five sub-projects (from the "Head First iPhone and iPad Development" book):

1. Open any sub-project's `.xcodeproj` file (Hello World, iDecide, DrinkMixer, InstaTwit, iBountyHunter)
2. Select an iOS Simulator target
3. Build and run (`Cmd+R`)

> **Note:** Modern Xcode versions may require disabling ARC or adding `-fno-objc-arc` compiler flags since all sub-projects use manual reference counting.
