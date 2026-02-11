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

## VoiceOver Testing

- Test apps with **VoiceOver enabled** to ensure usability for visually impaired users
- Navigate through **all key screens** to verify:
  - Accessibility labels are present
  - Reading order is logical and intuitive
- Use **Xcode Accessibility Inspector** to:
  - Identify missing accessibility labels
  - Verify accessibility traits and roles
- Pay special attention to **custom UI components**:
  - Custom views are not announced correctly by default
  - Explicitly set `accessibilityLabel` and `accessibilityTraits`
- Use **Screen Curtain** during testing to simulate full blindness
- Ensure the app remains fully usable **without visual feedback**

---

## Dynamic Type

- Allows users to **adjust preferred font size system-wide**
- Supported by default in **SwiftUI**
- Best practices:
  - Use system text styles (`.body`, `.headline`, etc.)
  - Avoid hard-coded font sizes
- Ensures readability across all accessibility text size settings

---

## Core Accessibility Challenges

- Accessibility is about **inclusive usability**, not just VoiceOver support
- Common user needs to consider:
  - Visual impairments
  - Hearing impairments
  - Color blindness
  - Limited motor control
- Key problems to solve:
  - Text scaling with Dynamic Type
  - Avoiding reliance on color alone
  - Providing sufficiently large tap targets
  - Adding accessibility support for custom views
- Most accessibility issues stem from **design awareness**, not technical complexity
- Small UI decisions can have a **significant real-world impact**

---

## Accessibility Accommodations in Practice

- Avoid using color as the only means of conveying information:
  - Combine color with icons or text
- Enable **Dynamic Type** throughout the app
- Respect system accessibility settings:
  - Adjust or disable animations when **Reduce Motion** is enabled
- Add VoiceOver support for custom UI:
  - Accessibility labels
  - Accessibility traits
  - Logical element grouping
- Prefer system components and text styles where possible
- Accessibility improvements:
  - Are sometimes trivial
  - Sometimes require UI or animation rework
  - Consistently improve the experience for all users

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

## What is the Role of Clean Architecture in Swift?

Clean Architecture is a way of organizing your app code so that each part (UI, logic, and data) is clearly separated. The idea is to make your codebase easier to understand, test, and scale, especially as your app grows.

### Layered Structure:

1. Presentation Layer (UI)

- Contains ViewControllers, SwiftUI views, or Composables.
- Talks to the domain layer to get data and show it to the user.

2. Domain Layer (Business Logic)

- Contains Use Cases (what your app does).
- Independent of frameworks like UIKit or SwiftUI.
- Pure Swift code, easy to test.

3. Data Layer (Repositories / API / Database)

- Handles network requests, database access, etc.
- Sends data to Domain layer via interfaces (protocols).

### Benefits:

- Testability

You can test business logic without worrying about UI or API.

- Separation of Concerns

Each layer has a specific job. UI doesn’t handle business logic. Business logic doesn’t care how data is fetched.

- Scalability

Works great for large teams and big apps. Each developer can focus on a specific layer.

- Maintainability

If you want to replace the API or change UI framework, you can do it without affecting core logic.

### Example Flow:

ViewController → calls UseCase (business rule) → which talks to Repository (data) → which calls API or DB → returns result to UseCase → updates UI.

## When to use Factory design pattern in Swift?

When object creation is complex or depends on conditions.

```
protocol Notification {}
class Email: Notification {}
class SMS: Notification {}

class NotificationFactory {
   static func create(type: String) -> Notification {
      return type == "email" ? Email() : SMS()
   }
}
```

## How would you explain delegates to a new Swift developer?

Delegation allows you to have one object act in place of another, for example your view controller might act as the data source for a table. The delegate pattern is huge in iOS, such as UITableViewDelegate from UIKit.

## Can you explain MVC, and how it's used on Apple's platforms?

MVC is an approach that advocates separating data (model) from presentation (view), with the two parts being managed by separate logic (a controller). In theory this separation should be as clear as possible, how ever view controllers sometimes get bloated as code gets merged together into one big blob.

## Can you explain MVVM, and how it might be used on Apple's platforms?

**MVVM** stands for *Model–View–ViewModel*.

- **Model** represents your data and business logic.
- **View** is your UI layer — the part that the user sees and interacts with.
- **ViewModel** acts as the middle layer that holds your app’s state and exposes data in a way that’s easy for the View to consume.

MVVM separates concerns between data, UI, and state.

The Model holds the data, the View shows it, and the ViewModel connects the two.

On Apple platforms like SwiftUI, the View binds to data published by the ViewModel.

Networking is usually handled in a Repository layer injected into the ViewModel.

## Can you explain MVI, and how it might be used on Apple's platforms?

1. Core Concept (The Definition)
	- MVI stands for Model-View-Intent.
	- It is a Unidirectional Data Flow (UDF) architecture.
	- The Loop: User Action → Intent → State Update → Model → UI Update → View.

2. MVI on Apple Platforms (SwiftUI)
	- State-Driven: Perfectly matches SwiftUI's View = f(State) philosophy.
	- Single Source of Truth: All UI data is stored in one single State struct (instead of multiple scattered variables).
	- Intent Handling: Users send "Intents" (Actions) to a Store or ViewModel, which processes logic and updates the state.

3. Why choose MVI over MVVM? (Pros)
	- Predictability: Since state only changes in one place, it is easy to debug and track.
	- No "Illegal" States: Prevents bugs like showing a "Loading Spinner" and "Error Message" at the same time.
	- Testability: Business logic is a "pure function" (Input Action + Old State = New State), making unit tests very simple.

4. The Trade-offs (Cons)
	- Boilerplate: Requires more code (Enums for Actions, Structs for State) even for simple features.
	- Learning Curve: Concepts like "Immutability" and "State Streams" can be harder for beginners than simple Data Binding.

## How would you explain protocol-oriented programming to a new Swift developer?

It is a way of designing code around **what something can do**, rather than **what it is**.

In object-oriented programming, we usually use inheritance to share behavior – which can lead to deep class hierarchies. In protocol-oriented programming, we use **protocols and extensions** instead, so we can define behaviors in a horizontal way.

One of the biggest advantages is that protocols work not just with classes, but also with structs and enums. That means we can get many of the benefits of OOP – like code reuse and polymorphism – while still taking advantage of Swift’s value types.

POP is really about **composition over inheritance**, and it fits perfectly with Swift’s design philosophy.

```
import Foundation

// 1. The Protocol: Defines the "capability" rather than the "identity"
protocol Transportable {
    var travelMode: String { get }
    func move(to destination: String)
}

extension Transportable {
    // Default implementation: Not all types need to write their own version of this
    func checkIn() {
        print("Candidate has arrived at the gate.")
    }
}

// 2. Struct Implementation (Value Type)
struct Bus: Transportable {
    let routeNumber: Int
    var travelMode: String { "Bus No.\(routeNumber)" }
    
    func move(to destination: String) {
        print("Taking the \(travelMode) to \(destination).")
    }
}

// 3. Enum Implementation (Value Type / State Machine)
enum CommuteStyle: Transportable {
    case car(isElectric: Bool)
    case cycling
    
    var travelMode: String {
        switch self {
        case .car(let isElectric): 
            return isElectric ? "Electric Car" : "Gas Car"
        case .cycling: 
            return "Bicycle"
        }
    }
    
    func move(to destination: String) {
        print("Commuting via \(travelMode) to \(destination).")
    }
}

// 4. The "Consumer" of the protocol
class InterviewProcessor {
    // This method demonstrates POP: It accepts any type conforming to Transportable
    func conductInterview(for candidateName: String, method: Transportable) {
        print("--- Interview Session Started ---")
        print("Candidate: \(candidateName)")
        method.move(to: "Headquarters Room 101")
        print("Arrival confirmed via \(method.travelMode).")
    }
}

// --- Usage ---

let interviewer = InterviewProcessor()

let candidateA = Bus(routeNumber: 42)
let candidateB = CommuteStyle.car(isElectric: true)

interviewer.conductInterview(for: "John", method: candidateA)
interviewer.conductInterview(for: "Alice", method: candidateB)
```

##  How would you explain dependency injection to a junior developer?

Dependency injection is the practice of creating an object and telling it what data it should work with, rather than letting that object query its environment to find that data for itself. Although this goes against the OOP principle of encapsulation, it allows for mocking data when testing, for example.

1. Constructor Injection:
Dependencies are passed through the initializer (init).

```
class UserManager {
    let apiService: APIService

    init(apiService: APIService) {
        self.apiService = apiService
    }
}
```

Now, you can inject a different APIService (e.g. mock or real).

2. Property Injection:
Dependencies are set via properties after initialization.

```
class UserManager {
    var apiService: APIService?
}
```

Benefits:

- Makes classes less dependent on specific implementations.
- Supports unit testing (you can pass mocks/stubs).
- Encourages cleaner architecture.

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

## Why must UI updates be performed on the main thread?

1. The Foundation (Legacy & UIKit) UIKit is not thread-safe. All drawing, layout, and event handling are tied to the Main RunLoop.

	- The Risk: Updating from background threads causes "Race Conditions," leading to corrupted memory, flickering UI, or crashes.
	- The Manual Way: Developers traditionally used DispatchQueue.main.async to jump back to the main thread.

2. The SwiftUI Reality Even though SwiftUI is newer, it still renders through Core Animation. When you update @State or @Published properties, SwiftUI must reconcile the view tree on the main thread to keep the screen in sync with the data.

3. The iOS 17 Evolution (@MainActor) In iOS 17, the responsibility has shifted from the developer’s "manual check" to the compiler’s "automatic enforcement."

	- The Tool: We now use the @MainActor macro.

	- The Shift: Instead of manually wrapping code in blocks, you mark your classes (like those using the new @Observable macro) as @MainActor.

	- The Benefit: Swift now catches threading errors at compile-time. If you try to update UI data from a background thread, the app won't even compile, preventing bugs before they happen.

## What are the practical pros and cons of using Singletons?

- The Pros: Why use them?

Single Source of Truth (SSOT): Ensures consistency for unique resources (e.g., UserDefaults.standard, Logging).

Convenience: Provides easy global access without passing objects through every layer.

- The Cons: Why are they bad for testing?

State Pollution: Global state persists between tests, causing one test to "poison" the next.

Hidden Coupling: Hard-coded calls (e.g., API.shared.fetch) make it impossible to swap real services for Mocks.

- The Solution: Dependency Injection (DI) Instead of accessing the singleton directly inside a class, inject it through the initializer.

Before (Bad): let user = AuthService.shared.currentUser (Hard-coded, untestable)

After (Good - DI): init(authService: AuthService = .shared)

Production: Uses the default .shared instance.

Testing: You can inject a MockAuthService to simulate different scenarios.


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

## Experience level with XCTest and UI testing?

Deeply integrated with XCTest and actively utilizing the new Swift Testing framework. The architecture relies on unit testing for logic verification, supplemented by targeted UI tests for end-to-end validation of key features.

The strategy treats UI tests as high-value but high-maintenance assets, keeping them limited to critical paths to ensure the CI pipeline remains fast and reliable. Recent updates leverage the @Test syntax for enhanced readability and better test grouping.

# Performance

## How does memory management work in iOS? What is ARC?

- iOS uses Automatic Reference Counting (ARC) to manage memory.
- ARC keeps track of how many strong references are pointing to an object.
- When the reference count drops to zero, the memory is automatically freed.

Key Points:

- Strong reference: Increases reference count.
- Weak reference: Doesn’t increase count; used to avoid retain cycles.
- Unowned reference: Similar to weak, but assumes the object will never be nil.

Scenario Example:

If ViewController strongly owns a delegate, and the delegate also strongly references the ViewController, a retain cycle happens. To fix it, delegate should be marked as weak.

## Explain the difference between escaping and non-escaping closures.

- Non-escaping closure: Called immediately within the function’s body. Doesn’t outlive the function.
- Escaping closure: Stored and called later. Might outlive the function scope.

Use @escaping when:

- Closure is passed to an async API or stored for later use.

Scenario:
In a network call using URLSession, the completion handler is escaping, because the response comes later.

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

## How does GCD work? What is the difference between async and sync?

GCD (Grand Central Dispatch) helps in multi-threading.

- DispatchQueue.main.async {} → for UI updates
- DispatchQueue.global().async {} → for background work
- async → doesn't block the current thread
- sync → blocks the thread until task completes

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

## What’s the difference between @State, @Binding, @ObservedObject, @EnvironmentObject in SwiftUI?

- @State: local state within a view
- @Binding: share state between parent and child view
- @ObservedObject: observed model passed into a view
- @EnvironmentObject: global shared data injected from environment

## What's the difference between @Bindable and @Binding? 

### @Binding (The Classic Bridge)
@Binding is used when a child view needs to read and write a value that is owned by a parent view. It doesn’t store the data itself; it just points back to the original source.

Used for: Simple data types (Strings, Bools, Ints) or passing a specific property.

The Vibe: "I don't own this, but I have permission to change it."

```
struct ToggleView: View {
    @Binding var isOn: Bool // Received from parent

    var body: some View {
        Toggle("Switch", isOn: $isOn)
    }
}
```

### @Bindable (The New Observable Tool)
Introduced with the Observation framework (iOS 17+), @Bindable is used specifically with classes marked with @Observable. It allows you to create bindings to the properties of an object that was passed into a view (usually via a parameter or environment).

Used for: Objects marked with @Observable.

The Vibe: "This object is already being watched; I’m just making its properties editable in a Form or TextField."

```
@Observable class UserProfile {
    var name = "Gemini"
}

struct ProfileEditView: View {
    var user: UserProfile // Just a standard property

    var body: some View {
        // We use @Bindable here to create a '$' binding on the fly
        @Bindable var bindableUser = user
        TextField("Name", text: $bindableUser.name)
    }
}
```

- Use @Binding if you are building a reusable component (like a custom slider) that needs to change a single piece of data.

- Use @Bindable if you have a data model (a class) and you need to plug its properties into things like TextField, Toggle, or Picker.

## What’s the difference between @ObservedObject, @State, and @EnvironmentObject?

- Use @State for simple properties that belong to a single view. They should usually be marked private.
- Use @ObservedObject for complex properties that might belong to several views. Most times you’re using a reference type you should be using @ObservedObject for it.
- Use @StateObject once for each observable object you use, in whichever part of your code is responsible for creating it.
- Use @EnvironmentObject for properties that were created elsewhere in the app, such as shared data.

# UIKit

## What are the best practices for embedding SwiftUI in a UIKit-based project, and embedding UIKit in a SwiftUI-based project?

General Principles

- Prefer module- or page-level integration, avoid fine-grained mixing
- Keep clear boundaries between UIKit and SwiftUI responsibilities

UIKit embedding SwiftUI

- Use UIHostingController to host SwiftUI views
- Typically embedded as a full page or independent feature module
- UIKit manages navigation and lifecycle
- Communicate via ViewModel, Combine, or callbacks
- Common approach for incremental adoption of SwiftUI

SwiftUI embedding UIKit

- Use UIViewControllerRepresentable / UIViewRepresentable
- Mainly for unsupported or advanced UIKit features
- Encapsulate UIKit as a self-contained component
- SwiftUI remains the source of truth for state and data