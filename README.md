# iOS-Interview-Questions

# Table of Contents

[Accessibility](https://github.com/jeuneseven/iOS-Interview-Questions?tab=readme-ov-file#accessibility)

[Data](https://github.com/jeuneseven/iOS-Interview-Questions?tab=readme-ov-file#data)

[Design patterns](https://github.com/jeuneseven/iOS-Interview-Questions?tab=readme-ov-file#design-patterns)

[Frameworks](https://github.com/jeuneseven/iOS-Interview-Questions?tab=readme-ov-file#frameworks)

[iOS](https://github.com/jeuneseven/iOS-Interview-Questions?tab=readme-ov-file#ios)

[Miscellaneous](https://github.com/jeuneseven/iOS-Interview-Questions?tab=readme-ov-file#miscellaneous)

[Performance](https://github.com/jeuneseven/iOS-Interview-Questions?tab=readme-ov-file#performance)

[Security](https://github.com/jeuneseven/iOS-Interview-Questions?tab=readme-ov-file#security)

[Swift](https://github.com/jeuneseven/iOS-Interview-Questions?tab=readme-ov-file#swift)

[SwiftUI](https://github.com/jeuneseven/iOS-Interview-Questions?tab=readme-ov-file#swiftui)

[UIKit](https://github.com/jeuneseven/iOS-Interview-Questions?tab=readme-ov-file#uikit)

# Accessibility

# Data

## How do you persist data in iOS?

- UserDefaults: small key-value data
- Core Data: complex structured data
- FileManager: for files
- Keychain: for secure data (passwords, tokens)

## What does the Codable protocol do?

- Purpose: A typealias for Encodable & Decodable, enabling a type to be converted to and from external representations like JSON or Property Lists.
- Key Requirement: Properties within the type must also be Codable.
- Use Case: Essential for API integration (parsing server responses) and local data persistence (saving data to disk or UserDefaults).
- Keywords: JSON, Serialization, Deserialization, JSONEncoder, JSONDecoder, Persistence.

## What does the Identifiable protocol do?

- Purpose: Ensures an instance has a stable, unique identity regardless of its value.
- Key Property: Requires a single property named id.
- Use Case: Critical for SwiftUI lists and collections to track which items are added, removed, or moved, ensuring smooth animations and data integrity.
- Keywords: Stable identity, id, SwiftUI reconciliation, Unique.

## What does the Hashable protocol do?

- Purpose: Allows an object to be converted into a hash value (an integer).
- Key Requirement: The object must also conform to Equatable.
- Use Case: Necessary for storing instances in a Set or using them as keys in a Dictionary. It enables fast $O(1)$ lookups.
- Keywords: Hash value, Set, Dictionary keys, $O(1)$ lookup, Deterministic.

## What does the Equatable protocol do? 

- Purpose: Defines a way to compare two instances for equality.
- Key Operator: Implements the == operator.
- Use Case: Used for basic logic checks (if a == b), searching in arrays (contains), or unit testing.
- Keywords: Comparison, == operator, Value equality, Logic branching.

## What does the Comparable protocol do?

- Purpose: Extends Equatable to allow objects to be ordered or ranked.
- Key Operator: Implements the < (less than) operator.
- Use Case: Essential for sorting arrays (.sorted()), finding the min() or max(), and using range operators (...).
- Keywords: Sorting, Relational operators (<, >), Ranking, Order.

## What does the CaseIterable protocol do?
- Purpose: Automatically generates a collection of all values in an enum.
- Key Property: Provides an allCases static property.
- Use Case: Perfect for populating Pickers, Segmented Controls, or looping through every possible state of an enum without hardcoding an array.
- Keywords: allCases, Enum iteration, UI populating, Reflection.

## What does the CustomStringConvertible protocol do? 

- Purpose: Allows you to define a custom text representation for an instance.
- Key Property: Requires a description string property.
- Use Case: Improves debugging and logging. Instead of a messy default output, print(object) returns the specific string you defined.
- Keywords: description, Debugging, Logging, String interpolation.

# Design patterns

##  How would you explain dependency injection to a junior developer?

Dependency injection is the practice of creating an object and telling it what data it should work with, rather than letting that object query its environment to find that data for itself. Although this goes against the OOP principle of encapsulation, it allows for mocking data when testing, for example.

##  What architecture would you use for a simple app vs a complex app?

- Simple app (few pages): MVC is sufficient - separates Model, View, Controller
- Medium complexity: MVVM - decouples UI from business logic, easier to test
- Large/complex app: Clean Architecture or VIPER - separates entities, interactors, presenters
- Key principle: Choose based on project scale. Over-engineering small apps creates unnecessary complexity.

## Have you used VIPER with SwiftUI? Any challenges?

- VIPER is heavy for SwiftUI apps
- SwiftUI naturally fits MVVM pattern
- For most SwiftUI apps, MVVM is sufficient and recommended
- VIPER better suited for UIKit-based large codebases (10,000+ lines)

# Frameworks

## How do you manage modularization?

- Use CocoaPods or SPM for foundation modules (networking, UI components)
- Feature modules organized in folders or as separate frameworks
- Deep link system allows each module to be independent
- Each module can use its own architecture (MVC/MVVM)
- Version management: semantic versioning (major.minor.patch)

# iOS

## Explain the iOS Application Lifecycle.

Key methods in AppDelegate or SceneDelegate:

- application(_:didFinishLaunchingWithOptions:) – app launched
- applicationDidEnterBackground(_:) – app moved to background
- applicationWillEnterForeground(_:) – app returning to foreground
- applicationDidBecomeActive(_:) – app is active
- applicationWillTerminate(_:) – app is terminating

Used to manage resources, save data, pause/resume tasks.

# Miscellaneous

## How does GCD work? What is the difference between async and sync?

- DispatchQueue.main.async {} → for UI updates
- DispatchQueue.global().async {} → for background work
- async → doesn't block the current thread
- sync → blocks the thread until task completes

# Performance

## How do you avoid retain cycles?

- Use weak or unowned for delegate references
- Use [weak self] in closures
- Be careful with closures in view models
- Instruments/Memory Graph Debugger to detect leaks

## Difference between weak, strong, and unowned in Swift?

- strong: default, keeps the object in memory
- weak: doesn’t increase retain count, allows ARC to deallocate (used in delegates, optional)
- unowned: like weak but non-optional, used when object will always be in memory

Avoid retain cycles using weak in closures or delegates.

# Security

# Swift

## What's the difference between var and let?

- let = constant (cannot change)
- var = variable (can change)
- Best practice: Use let by default, var only when mutation needed
- Compiler will warn if var is never mutated

## What is the difference between struct and class in Swift?

- Struct is a value type, copied on assignment.
- Class is a reference type, shared on assignment.
- Struct is preferred for immutability and thread safety.
- Use struct when you want a copy; use class when you need shared state or inheritance.

## What does the question mark (?) mean in Swift?

- Indicates Optional type
- Value can be nil (absent)
- Used when data might not exist (e.g., optional JSON fields)
- Must unwrap before use (optional binding, guard, force unwrap)

## Explain Optionals and their safety features

- Swift's way to handle absence of value
- Prevents null pointer crashes
- Unwrapping methods:
	- Optional binding: if let, guard let
	- Nil coalescing: ??
	- Optional chaining: ?.
	- Force unwrap: ! (avoid when possible)

# SwiftUI

## What's the difference between @Bindable and @Binding? 

## What’s the difference between @ObservedObject, @State, and @EnvironmentObject?

- Use @State for simple properties that belong to a single view. They should usually be marked private.
- Use @ObservedObject for complex properties that might belong to several views. Most times you’re using a reference type you should be using @ObservedObject for it.
- Use @StateObject once for each observable object you use, in whichever part of your code is responsible for creating it.
- Use @EnvironmentObject for properties that were created elsewhere in the app, such as shared data.

# UIKit