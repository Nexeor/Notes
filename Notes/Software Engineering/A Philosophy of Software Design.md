2025-05-20 16:51

Status: #Trunk

Tags: #ProgrammingConcepts 

# Deep Modules
**Module**: A self-contained piece of code with a specific functionality
- Can be classes, services, syscalls, etc.
### Interfaces and Implementation
All modules have two components, **interface** and **implementation**
- **Interface**: Everything that a developer working with the module must know to use it
- **Implementation:** The code that accomplishes what's specified in the interface
- The **interface** is *what* the module does, the **implementation** is *how* it does it
- Ex: The interface of a linked list is the add/modify methods the programmer invokes, the implementation is the complex logic that manages the data within the structure

The best modules have **interfaces that are simpler than their implementations**
- Simple interfaces minimize what you need to know to use it 
- Modifying a module's implementation without changing the interface doesn't affect other modules
Interfaces are composed of two types of information, **formal** and **informal**:
- **Formal** information is specified in the code and usually checked for correctness by the language
	- Names and types of params, return values, exceptions. etc.
- **Informal** information is high level behavior that cannot be understood just from the code
	- Can only be described in comments and documentation
	- Side effects, how to use an object, the order to call functions, etc.
	- Poorly documented informal interfaces are typically responsible for creating "unknown unknowns"

**Abstraction:** A simplified view of an entity that omits unimportant details for clarity
- The interface of a module is an abstraction of that module's implementation
A good abstraction has **includes only the most important details**
- An abstraction with too many details makes the abstraction complex
- **False Abstraction**: An abstraction with too few details, making the interface appear simpler than it is
- Ex: A file system shows you the size, name, and path of files. But it hides details like caching, write buffering, choosing blocks, etc. 

The best modules are **deep**: They have simple interfaces that hide complex functionality
- Deep modules are easy to learn, but are also highly useful
- Sometimes the best thing we can do is removing or shrinking an interface to make it easier to use
- Shallow modules are the opposite: they have complex interfaces that accomplish simple functionality. These should be avoided!

## References
