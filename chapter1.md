# Software Design
This section aims to outline some general best practices for Object

## Components and Boundaries
Component based software development emphasises the separation of concerns. Software projects contain small components that interact with each other. Each component should address a specific concern (high cohesion) and be independent of other components (loose coupling).

**What does this mean and why is it important?**
The purpose of this it to think of the piece of software in smaller and smaller parts. A feature may be considered a component and there are components within the feature that work together to deliver the feature as a whole.

A good example of a component is a software package. The package is an independent component that is used by other pieces of software. The component itself is not coupled to any other piece of software. Software that uses the component should aim to loosely couple with the software in the package. This means the package can be replaced easily.

The critical part of creating components is well defined boundaries for components. To create well defined boundaries components should aim to have high cohesion and be loosely coupled. Components with high cohesion focus on one particular small task. Components with loose coupling have few dependencies on other components. High cohesion makes components easier to understand as they focus on a few small tasks. Loose coupling makes code easier to change and maintain. Tightly coupled components have huge ripple effects when they are changed, i.e. changing x requires a change to y followed by changes to z and so on. 


