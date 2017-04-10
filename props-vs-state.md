_props_ vs _state_
======

Ever since we started using [React](http://facebook.github.io/react/) to rebuild our UI at [uberVU](https://www.ubervu.com/) (now [Hootsuite](https://hootsuite.com/)) the #1 developer question has probably been: 

> What's the exact difference between _props_ and _state_?

It's fairly easy to understand how they work—especially when seen in context—but it's also a bit difficult to grasp them conceptually. It's confusing at first because they both have abstract terms and their values look the same, but they also have very different _roles._ 

### Context

The main responsibility of a Component is to translate raw data into rich HTML. With that in mind, the _props_ and the _state_ together constitute the _raw data_ that the HTML output derives from.

You could say _props_ + _state_ is the input data for the `render()` function of a Component, so we need to zoom in and see what each data type represents and where does it come from.

Because we also use [Cosmos](https://github.com/skidding/cosmos) where _props_ can contain an initial _state_, getting this straight is crucial.

### Common ground

Before separating _props_ and _state_, let's also identify where they overlap.

- Both _props_ and _state_ are **plain JS objects**
- Both _props_ and _state_ changes trigger a **render update**
- Both _props_ and _state_ are **deterministic.** If your Component generates different outputs for the same combination of _props_ and _state_ then you're doing something wrong.

### Does this go inside _props_ or _state_?

**tl;dr:** If a Component needs to alter one of its attributes at some point in time, that attribute should be part of its _state_, otherwise it should just be a _prop_ for that Component.

#### _props_

_props_ (short for _properties_) are a Component's **configuration,** its _options_ if you may. They are received from above and **immutable** as far as the Component receiving them is concerned.

A Component cannot change its _props,_ but it is responsible for putting together the _props_ of its child Components.

#### _state_

The _state_ starts with a default value when a Component mounts and then **suffers from mutations in time (mostly generated from user events).** It's a serializable* representation of one point in time—a snapshot.

A Component manages its own _state_ internally, but—besides setting an initial state—has no business fiddling with the _state_ of its children. You could say the state is **private.**

\* We didn't say _props_ are also serializable because it's pretty common to pass down callback functions through _props._ 

#### Changing _props_ and _state_

&nbsp; | _props_ | _state_ | 
--- | --- | --- 
Can get initial value from parent Component? | Yes | Yes
Can be changed by parent Component? | Yes | No
Can set default values inside Component?* | Yes | Yes
Can change inside Component? | No | Yes
Can set initial value for child Components? | Yes | Yes
Can change in child Components? | Yes | No

\* Note that both _props_ and _state_ initial values received from parents override default values defined inside a Component.

### Should this Component have _state_? 

_state_ is optional. Since _state_ increases complexity and reduces predictability, a Component without _state_ is preferable. Even though you clearly can't do without state in an interactive app, you should avoid having too many _Stateful Components._

#### Component types

* **Stateless Component** — Only _props_, no _state._ There's not much going on besides the `render()` function and all their logic revolves around the _props_ they receive. This makes them very easy to follow (and test for that matter). We sometimes call these _dumb-as-f*ck Components_ (which [turns out](http://www.urbandictionary.com/define.php?term=dumb%20as%20fuck) to be the only way to misuse the F-word in the English language).
* **Stateful Component** — Both _props_ and _state._ We also call these _state managers_. They are in charge of client-server communication (XHR, web sockets, etc.), processing data and responding to user events. These sort of logistics should be encapsulated in a moderate number of _Stateful Components_, while all visualization and formatting logic should move downstream into as many _Stateless Components_ as possible.

### Sources of truth

- [Question about 'props' and 'state' - Google Groups](https://groups.google.com/forum/#!topic/reactjs/hAldztPzQgI)
- [Thinking in React: Identify where your state should live](http://facebook.github.io/react/docs/thinking-in-react.html#step-4-identify-where-your-state-should-live)
