# AGENTS.md

This file provides guidance to AI coding agents (Claude Code, Codex, Cursor, GitHub Copilot) when working with code in this repository.

## What this repo is

Five standalone iOS practice apps in Objective-C, from the "Head First iPhone and iPad Development" book: `Hello World`, `iDecide`, `DrinkMixer`, `InstaTwit`, `iBountyHunter`. Development was discontinued on 2012-02-16; the repo is a preserved historical reference. A parallel guidance file exists for GitHub Copilot at `.github/copilot-instructions.md` — keep the two consistent in substance when editing either.

## Build, run, validate

There is **no build system, package manager, linter, or test suite** — do not look for `make`, `npm`, CI build steps, or a `Tests/` target; none exist. Each sub-project is a self-contained Xcode project.

- Build/run: open a sub-project's `.xcodeproj` in Xcode (e.g. `open "DrinkMixer/DrinkMixer.xcodeproj"`), pick an iOS Simulator target, `Cmd+B` / `Cmd+R`.
- Validation is **manual only**: build, run on the Simulator, exercise the UI. There is nothing to run from the command line.
- GitHub workflows (all delegating to `rios0rios0/pipelines`) never build or test the code; pull requests are gated by `checks.yaml`, which runs the shared rebase and changelog check.

## Conventions that differ from modern Objective-C

- **Manual reference counting (MRC), not ARC.** Code uses explicit `retain`/`release`/`autorelease` and releases in `dealloc`. Do not introduce ARC syntax. Modern Xcode may prompt to migrate to ARC — that breaks these targets; the workaround is `-fno-objc-arc` / disabling ARC in Build Settings.
- **XIB-based UI**, no programmatic layout and no storyboards. XIB names don't always match the controller: `DrinkMixer/AddViewController` has no XIB of its own and loads `DetailViewController.xib`, and Hello World's `ViewController2`–`6` load `View2`–`View6.xib`.
- Object properties use `(nonatomic, retain)`; delegates/primitives use `(nonatomic, assign)`. The Xcode 4.2 template code — `@autoreleasepool` in every `main.m`, `strong` on the `AppDelegate` properties — compiles under MRC; leave it as is.
- **`DrinkMixer/DrinkConstats.h`** — the filename is misspelled (the in-file comment reads `DrinkConstants.h`); keep the existing spelling, three sources `#import` it. It declares `#define` macros `NAME_KEY` (`@"name"`), `INGREDIENTS_KEY` (`@"ingredients"`), `DIRECTIONS_KEY` (`@"directions"`) — these are the dictionary/plist keys for every drink and for `DrinkDirections.plist`.

## Architecture notes worth knowing

- **DrinkMixer**: master/detail over an `NSMutableArray` of `NSDictionary` drinks; `AddViewController` is modal, has no delegate, and edits the `ArrDrinks` array it shares with `MasterViewController`, which reloads its table in `viewWillAppear:`. Drinks are read from and written back to the bundle's `DrinkDirections.plist` on app termination via an `NSNotificationCenter` observer — that works in the Simulator but not on a device, where the bundle is read-only; expected, do not move it to Documents.
- **InstaTwit**: posts via a **synchronous** `NSURLConnection sendSynchronousRequest:` to the retired Twitter v1 XML endpoint with credentials inlined in the URL. Network calls will fail today — expected, do not "fix."
- **iBountyHunter**: Core Data stack lives in `AppDelegate`; `NSFetchedResultsController` (with a named cache) drives the fugitive and captured lists; `Fugitive` is an `NSManagedObject` that also conforms to `MKAnnotation` (MapKit), so it carries `coordinate`/`title`/`subtitle`. Camera capture is in `CapturedPhotoViewController`. On first launch `AppDelegate` copies the bundled seed `iBountyHunter.sqlite` into Documents and opens it with lightweight migration across the model versions under `iBountyHunter.xcdatamodeld/`; delete the app from the Simulator before re-running only when a schema change is beyond lightweight migration or the bundled seed changed.

<!-- chlog:start -->
## Changelog (chlog) — MANDATORY

If the repository you are working in uses chlog (a `.chlog.yaml` or `.chlog.yml`
config file, or a `.changes/` directory, exists at the project root), the
following is binding and ALWAYS applies: whenever you make ANY change, you MUST
create a changelog fragment as part of the same change — automatically, without
being asked, before committing.

- Do NOT edit CHANGELOG.md directly; it is generated from fragments.
- Create the fragment with:
  `chlog new --kind <Kind> --body '<past-tense description>'`
- Write an apostrophe inside the single-quoted body as `'\''`.
- Valid kinds: Added, Changed, Deprecated, Removed, Fixed, Security
- Choose the kind that best matches the change (e.g., new feature → Added,
  bug fix → Fixed, behavior change → Changed, removal → Removed, security fix → Security).
- If the change is backward-INCOMPATIBLE with the public API (a breaking
  change), you MUST add the `--breaking` flag:
  `chlog new --kind <Kind> --breaking --body '<past-tense description>'`.
  This is the ONLY thing that triggers a major version bump — the kind alone
  never does (per SemVer, major = incompatible change). When unsure whether a
  change breaks compatibility, ask the user instead of guessing.
- Fragments are YAML files in `.changes/unreleased/`; stage them with your commit.
- `chlog check` fails the build when a fragment is missing — never skip it.
<!-- chlog:end -->
