title: Reselect Presentation
author:
  name: Philip Kubiak
  url: https://github.com/koobeeyak
output: basic.html
controls: true

--
# **Selectors in Redux**
<br>
## A quick look  👀  into selectors and the Redux-Reselect library
## *Sema4 Front-End Guild Meeting* ⛩ *May 9, 2019*

--

### Why do we use Redux?

--

### Why do we use Redux?

- *Application has a global state (the Redux store)*
<br>


--

### Why do we use Redux?

- *Application has a global state (the Redux store)*
	- *Can influence the state of any component*
<br>


--

[//]: # (Don't end up passing too many props down into distant children)

### Why do we use Redux?

- *Application has a global state (the Redux store)*
<br>


- *Separate the logic that decides **what** we should render away from the logic that decides **how** we should render it*

--

<br><br>

```js
// app/containers/ThisViewsContainer.js

import { connect } from 'react-redux';
import ThisViewsComponent from '../components/ThisViewsComponent';

```

--

<br><br>

```js
// app/containers/ThisViewsContainer.js

import { connect } from 'react-redux';
import ThisViewsComponent from '../components/ThisViewsComponent';

const mapStateToProps // = ...?
```

--

<br><br>

```js
// app/containers/ThisViewsContainer.js

import { connect } from 'react-redux';
import ThisViewsComponent from '../components/ThisViewsComponent';

const mapStateToProps = state => ({ /* ... */ });
```

--

<br><br>

```js
// app/containers/ThisViewsContainer.js

import { connect } from 'react-redux';
import ThisViewsComponent from '../components/ThisViewsComponent';

const mapStateToProps = state => ({ /* ... */ });
const mapDispatchToProps // = ...?
```

--

[//]: # (What is state in this context? What is dispatch?)

<br><br>

```js
// app/containers/ThisViewsContainer.js

import { connect } from 'react-redux';
import ThisViewsComponent from '../components/ThisViewsComponent';

const mapStateToProps = state => ({ /* ... */ });
const mapDispatchToProps = dispatch => ({ /* ... */ });
```

--

<br><br>

```js
// app/containers/ThisViewsContainer.js

import { connect } from 'react-redux';
import ThisViewsComponent from '../components/ThisViewsComponent';

const mapStateToProps = state => ({ /* ... */ });
const mapDispatchToProps = dispatch => ({ /* ... */ });

const ThisViewsContainer = connect(
	mapStateToProps,
	mapDispatchToProps
)(ThisViewsComponent);
```

--


<br><br>

```js
// app/components/ThisViewsComponent.js

const ThisViewsComponent = ({ statePassedViaProps, dispatchPassedViaProps }) => (
	<div>{ /* ... */ }<div>
)

export ThisViewsComponent;

```

--

### Why do we use Redux?

- *Application has a global state (the Redux store)*
<br>


- *Separate the logic that decides **what** we should render away from the logic that decides **how** we should render it*

--

### Separation of State & State

- *Develop with a strict differentiation between components that render stuff*
    - *Call* `render()`
- *From components that provide data and conditions that determine rendering (can call them containers)*
	- *Pass* `{ ...props }`

--

### The "Selector" Ideology

If you develop with...

&nbsp;&nbsp;&nbsp;*Presentational & Container Components,*

&nbsp;&nbsp;&nbsp;*Smart & Dumb(ish?) Components,*

&nbsp;&nbsp;&nbsp;*Stateful & Pure Components,*

... you should develop with **selectors.**

--

[//]: # (Argue for whether this tweet is outdated? React Hooks released as of v. 16.8, Sema4 is on v. 16.2)

## <img src="assets/dan-tweets.png" alt="Dan Tweets" height="622"/>

--

## <img src="assets/squidward-tweets.png" alt="Squidward Tweets" height="622"/>

--

<br><br>

`const mapStateToProps = state => ` **`({ /* ... */ })`**

--

[//]: # (Make the note that you can just pass the entire state to mapStateToProps, and it COULD work)
[//]: # (That does take advantage of Redux's separation of state and state, but not the rest of what Redux has to offer)

--