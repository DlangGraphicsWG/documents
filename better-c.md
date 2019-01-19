# Do we `-better-c` or not?

Pros:
- More inclusive to more users
- Accessible by foreign languages
- Available to resource limited systems
- Increase opportunity to become a de-facto standard image representation library
- There's something to be said for compile times

Cons:
- Potentially affects API design in a way that frustrates usability
- Some phobos functionality may need to be mirrored locally

Considerations:
- Our suite not need be a monolith; low-level inter-op packages may be `better-c` while higher level functions take full advantage of the std library
- Where tension exists, we may choose to present modern-d and better-c overload apis
- Tasteful and careful design may be possible to allow for the best of both worlds

Discussions points:
- p0nce: personally I still don't think we should avoid phobos or the runtime. Illusion of consensus right here!
- NX133t: if for some reason we were to decide gui to be betterC I would just run away
- Manu: I would like to justify large dependencies with substance before they're drawn in
- Manu: Packages that define API layers may specify different dependency sets


I think it's possible to have our cake and eat it too with careful design.  
Everyone should be vigilent, and make sure each new API doesn't violate their design sensibilities, and we can iterate to make sure designs feel great from all perspectives.
