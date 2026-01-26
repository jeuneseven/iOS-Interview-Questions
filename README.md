# iOS-Interview-Questions

# Table of Contents

# Accessibility

# Data

# Design patterns

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

# Miscellaneous

# Performance

## How do you avoid retain cycles?

- Use weak or unowned for delegate references
- Use [weak self] in closures
- Be careful with closures in view models
- Instruments/Memory Graph Debugger to detect leaks

# Security

# Swift

## What's the difference between var and let?

- let = constant (cannot change)
- var = variable (can change)
- Best practice: Use let by default, var only when mutation needed
- Compiler will warn if var is never mutated

# SwiftUI

# UIKit