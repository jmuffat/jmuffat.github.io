# Disposable Modules

## what is a module ?

In software architecture, the term module is a big vague and this is by design: the purpose is to simplify development by breaking a big problem into smaller ones. 

At the highest level, a module is simply a subset of a project. It has two parts: 
- **interface**: everything the other modules can use
- **implementation**: the actual code&data, which other modules cannot access|reference directly

## making modules disposable

In our context, we need to architect projects so that any module can be taken out and replaced by a new|different one, in the same way one might upgrade a turbocharger on a car. The implication is clear: interface specification is key. 

To generate the implementation, we need to assemble a prompt that provides the desired _interface_, the _interfaces_ of other modules we want to use, as well as instructions about any further _requirements_ we have about the implementation. The good thing is that all this is also what we would provide human developers for them to come up with the code by hand. 

This gives a simple module structure:
- **interface**: hand crafted code, specification of everything other modules can use
- **requirement**: plain english, specification of what the implementation should do and under which constraint
- **implementation**: the code (can be generated, hand crafted or any mix) 

If for each module, we store the _interfaces_ and the _requirements_, we can generate new _implementations_ on demand. When the _requirements_ for a module changes, we know we need to generate the _implementation_ again. When the _interface_ for a module changes, we know we to generate the _implementation_ again, as well as to check whether dependent modules are affected.

## module size

This way or working leads to an interesting tension regarding module size|granularity. 

- with larger modules, there is less design work and more leeway&opportunities for the implementer (natural or artificial)
- with smaller modules, a change in requirements will trigger minimal code generation, limiting the risk of regression

It seems natural to expect that a project will start by breaking down the project in just a few large modules and sudivide as progress is being made and modules solidify accordingly. 

## hierarchy of modules

From the above it is quite clear that it should be possible to define a higher level module as a group of lower level modules. This has the advantage of bringing some order to the project. 

More importantly, it makes it possible to tune code generation. In a first approach we looked at every module's implementation being generated separately. In reality, depending on the AI being used (now and in the future), the optimal amount of code it can manage will vary. We can assume that this amount will grow with time (because if it doesn't, our approach doesn't have a point). Planning for a hierarchy of modules from the start lets us introduce the notion of generating higher level modules by assembling a prompt combining the interfaces and requirements for the included modules.

