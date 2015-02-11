Where does my code go?
======
_aka component > mixin > lib_


Our source code is divided into three layers: `lib`, `mixins`, and 
`components`. This is also reflected in our folder structure:

```
|
+-- src
|   +-- scripts
|   |   +-- components
|   |   +-- lib
|   |   +-- mixins
|   +-- styles
|   |   +-- components
|   |   +-- lib
|   |   +-- mixins
+-- tests
|   +-- unit
|   |   +-- components
|   |   +-- lib
|   |   +-- mixins
```

## JS(X)

### Component

A component is a 
[React class](http://facebook.github.io/react/docs/top-level-api.html#react.createclass), 
revolving around a DOM output for a specific UI element. Page elements should
be split into as many components as possible, in order to reduce their scope. 
However, components should always remain autonomous and be able to render 
something by themselves.

Complexity: _Let's see what we have here..._

### Mixin

[React mixins](http://facebook.github.io/react/docs/reusable-components.html#mixins)
are designed to own reusable component logic. They get merged into the
prototype of a React classes, but have a restricted scope and are easier to
test in isolation with the component API mocked.

Complexity: _I see what you did there_

### Lib

A lib is 100% oblivious of the React component API. Our libs are made out of
numerous stateless functions that don't want to have anything to do with 
`this`. They simply receive a (reasonable) number of arguments and return
something in response. They are the most favorable because they are very 
predictable and testing them is a joy.

Complexity: _Taking candy from a baby_

### So where does my code go?

1. Strive to put as much as possible into libs.
2. If something requires the component context, make it a mixin.
3. If something is related to a specific UI element, put it in a component.

This avoids duplication and keeps things as simple as possible.

## CSS/LESS

This structure was designed around React components and JavaScript in general, 
but it can easily be adapted to styles as well. We gain more from having 
consistency between both logic and style than from optimizing the structure of 
the styles separately.

The component domain is easy. Each component class has a corresponding style
file with the same name. The entire style is owned under a CSS class with the
same name, only with hyphens instead of camel case.

Since we use LESS, the mixin/lib part is confusing, because they're both
technically _mixins._ But even though they are the same type of entity, they 
are split by responsibility.

### So where does my style go?

1. If you're styling a reusable element from the UI design, make a lib out of it.
2. If the reusable style contains generic functionality–like a formula for border radius, or something you would open source–put it in a mixin. These are usually functions with parameters.
3. If the style belongs to one component only, put it in that compoenent's corresponding style file.
