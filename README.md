# Mithril v3 Redesign

TODO: fill in some details

https://github.com/MithrilJS/mithril.js/issues/2278#issuecomment-442003421

- [Core changes](core.md)
- [Vnode structural changes](vnode-structure.md)
- [Utilities added to `mithril/*`][utils.md]

The general idea is this:

- Code should be concise, flexible, and easily interpreted and modified. Writing generic utilities should be *easy*, almost thoughtless.

- The simple should be easy, and the complex possible. Preferably, the complex should also be easy and easy to do right.

- You should be able to feel how idiomatic the code is:
    - If it's a good idea, it should feel natural and be what you'd write without hardly thinking about it. For example, it just feels natural to write `m(".foo", ...)` or a simple function component.
    - If it's a bad idea, it should feel wrong and be clearly awkward to do. For example, passing `history` and other similar global context around *everywhere* feels clearly wrong compared to just reading what you need and passing individual parameters as necessary.
    - If it's a necessary hack, it should feel like a necessary hack, something you would almost certainly feel compelled to justify, maybe even with a comment. For example, `ref: elem => ...` should make you feel it's clearly a hack because you can't just use attributes.

- Mithril's npm package should be somewhat batteries-included, but not the core itself. You pick and choose what you want, or you can just use the core bundle for simple stuff.

- Performance and memory are not forgotten about. For example:
    - Attributes and children do *not* get retained unless you do something on your end. A significant portion of memory overhead come from attributes retaining a lot of data.
    - A strong preference for raw functions itself avoids method lookup overhead, resulting in faster calls.
    - Fewer entry points results in fewer polymorphic calls and more opportunity to optimize.
    - A single code path in components for compare, update, and return view means local variables are much more often used to compare, something engines optimize better.
