# TTD
## Why ?
- Because the civilization depends on us
- We can clean code with confidence
- Code will be testable which makes it decoupled

Double <- Dummy <- Stub <- Spy <- Mock
Double <- Fake

- Dummy: Do nothing. Example: return null in authenticate
- Stub: .Example: return True or False in authenticate
- Spy: Spies on how the object was used. Example: know how much time authenticate was called
- Mock: Babahom ga3, he does spies but knows what to expect from using the object.
- Fake: A simple simulator. Example: authenticate only one defined user.

