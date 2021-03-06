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

[//]: # (refresher)

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

[//]: # (additional points not written down, nice benefit about using redux you don't have pass props throughout children, don't have logical components, you have logicless ones)
[//]: # (almost following a modular sort of approach to component design)
[//]: # (I haven't coded a ton on React without redux, but..)
[//]: # (as soon as that happens you have components that are prime candidates for resusable components)
[//]: # (with this, let's go back, lets talk about the second point)

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

[//]: # (introduce the general ideas of selectors here, that they can be just JS methods that pass basic data points)

<br>

`const mapStateToProps = state => ` **`({ /* ... */ })`**

--

[//]: # (Make the note that you can just pass the entire state to mapStateToProps, and it COULD work)
[//]: # (That does take advantage of Redux's separation of state and state, but not the rest of what Redux has to offer)
[//]: # (We're going to be looking at breaking up that pass of the huge state via selectors)

<br><br>
```js
const mapStateToProps = state => ({ state: ...state.MyBigAndImportantReducer })
```

--

<br><br>
```js
const mapStateToProps = state => ({ state: ...state.MyBigAndImportantReducer })
```

<br><br>
```js
const mapStateToProps = state => ({
	someData: selectSomeData(state.MyBigAndImportantReducer)
})
```

--

[//]: # (Argue for whether this tweet is outdated? React Hooks released as of v. 16.8, Sema4 is on v. 16.2)

## <img src="assets/dan-tweets.png" alt="Dan Tweets" height="622"/>

--

## <img src="assets/squidward-tweets.png" alt="Squidward Tweets" height="622"/>

--

[//]: # (A use case where more users might be loaded at any moment)
[//]: # (I want to keep going from this example, passing whole state in, into an example with selectors and then reselect)

```js
// MyBigAndImportantReducer
{
	filters: { testResultsReleased: false, openedEmail: undefined },
	data: {
		fetchInProgress: true, errorMessage: undefined, successMessage: undefined,
		users: {
			0: { testResults: [{/* */}, {/* */}], name: 'Jared Guy', email: '...' },
			1: { testResults: [{/* */}, {/* */}], name: 'Emily Person', email: '...' }
		}
	}
}

```

--

[//]: # (Re-renders when user selects new filter or loads new users)
```js
// app/containers/DisplayUsersContainer.js

const mapStateToProps = state => ({ state: ...state.MyBigAndImportantReducer })
```
<br>
```js
// app/components/DisplayUsers.js

filterUsers(filters, users) {
	/* return a list of users narrowed down by filters */
}

render() {
	return (
		<div>
			{...this.filterUsers(this.state.filters, this.state.data.users)}
		</div>
	);
}
```

--

[//]: # (Let's talk about how we can rate this, we can talk about basic, most common ways to rate with readability & necessity to edit it in the future quickly with future changes which we can't predict)
[//]: # (Can rate it more specific to following the ideas about Redux we set forth and agreed upon earlier)
[//]: # (If you don't want to end up passing too many props down into distant children, using Redux to get around this, leaves me to believe at the very least we can make some further improvements)


# 🤷‍♂️ &nbsp; 🤷‍♀️

--

```js
// app/containers/DisplayUsersContainer.js

const mapStateToProps = state => ({
	filters: state.MyBigAndImportantReducer.filters,
	users: state.MyBigAndImportantReducer.data.users
})
```
<br>
```js
// app/components/DisplayUsers.js

filterUsers(filters, users) { /* will return a list of users by filter */ }

render() {
	return (<div>{...this.filterUsers(this.state.filters, this.state.users)}</div>);
}
```

--

[//]: # (I don't know how I feel about this, in fact I think I feel worse)
[//]: # (that's because we went back and tried to optimize here but we can't re-use those mapStateToProps calls)
[//]: # (our first look at some selectors which are simple, they can live right in the reducer file they will query from, or can always separate them out)

```js
// app/reducers/MyBigAndImportantReducer.js

export const selectDisplayFilters = state => state.filters;
export const selectUsers = state => state.data.users;
```

<br>

```js
// app/containers/DisplayUsersContainer.js

import { selectUsers, selectDisplayFilters } from '../reducers/MyBigAndImportantReducer';

const mapStateToProps = state => ({
	filters: selectDisplayFilters(state.MyBigAndImportantReducer),
	users: selectUsers(state.MyBigAndImportantReducer)
})
```

[//]: # (and the DisplayUsersComponent.js doesn't need to change)

--

[//]: # (I hope that it has piqued your interest at least)
[//]: # (filterUsers is just to take the example of what we had in component, arbitrary, can also imagine as something as.. calling filter)
[//]: # (this is really the way we should imagine a DisplayUsers component, isolated from shape of store)

```js
// app/reducers/MyBigAndImportantReducer.js

const selectDisplayFilters = state => state.filters;
const selectUsers = state => state.data.users;

export const filteredUsers = state =>
	filterUsers(selectDisplayFilters(state), selectUsers(state));
```

--

```js
// app/reducers/MyBigAndImportantReducer.js

const selectDisplayFilters = state => state.filters;
const selectUsers = state => state.data.users;

export const filteredUsers = state =>
	filterUsers(selectDisplayFilters(state), selectUsers(state));
```
<br>
```js
// app/containers/DisplayUsersContainer.js

const mapStateToProps = state => ({ users:  filteredUsers(state.MyBigAndImportantReducer)});
```

--
```js
// app/reducers/MyBigAndImportantReducer.js

const selectDisplayFilters = state => state.filters;
const selectUsers = state => state.data.users;

export const filteredUsers = state =>
	filterUsers(selectDisplayFilters(state), selectUsers(state));
```
<br>
```js
// app/containers/DisplayUsersContainer.js

const mapStateToProps = state => ({ users:  filteredUsers(state.MyBigAndImportantReducer)});
```
<br>
```js
// app/components/DisplayUsers.js

render() {
	return (<div>{...this.users}</div>);
}
```

--

<br><br><br>
## export const selectData = ( ... )

--

<br><br><br>
## ~~export~~ const selectData = ( ... )

--

[//]: # (essentially another selector)
[//]: # (this is where I want to start talking about reselect)

```js
// app/reducers/MyBigAndImportantReducer.js

const selectDisplayFilters = state => state.filters;
const selectUsers = state => state.data.users;

export const filteredUsers = state =>
	filterUsers(selectDisplayFilters(state), selectUsers(state));
```
<br>
```js
// app/containers/DisplayUsersContainer.js

const mapStateToProps = state => ({ users:  filteredUsers(state.MyBigAndImportantReducer)});
```
<br>
```js
// app/components/DisplayUsers.js

render() {
	return (<div>{...this.users}</div>);
}
```

--

## <img src="assets/reselect.png" alt="Reselect" height="262"/>
<br><br>
## <img src="assets/reselect-github.png" alt="Reselect GitHub" height="72"/>

--

#### From [Reselect Docs](https://github.com/reduxjs/reselect)

- *Selectors can compute derived data, allowing Redux to store the minimal possible state.*
- *Selectors are efficient. A selector is not recomputed unless one of its arguments changes.*
- *Selectors are composable. They can be used as input to other selectors.*

[//]: # (a few memoization packages out there for js, basically the memoization here is a cache for the function, memoization package usually brings an algorithm, deterministic one to determine when to update cache)

--

[//]: # (takes an array of selectors as first arg and a transform function as second)
[//]: # (but it's really simpler than that)


#### From [Reselect Docs](https://github.com/reduxjs/reselect)

- *Selectors can compute derived data, allowing Redux to store the minimal possible state.*
- *Selectors are efficient. A selector is not recomputed unless one of its arguments changes.*
- *Selectors are composable. They can be used as input to other selectors.*

#### calls to → `createSelector()`
--
<br>

#### Reselect API's &nbsp; `createSelector()`
- arg: *array of selectors*
- arg: *transform func*

--

[//]: # (again, can imagine simpler filtering for filter users)

```js
// app/reducers/MyBigAndImportantReducer.js

const selectDisplayFilters = state => state.filters;
const selectUsers = state => state.data.users;

export const filteredUsers = state =>
	filterUsers(selectDisplayFilters(state), selectUsers(state));
```
↓
<br>
```js
// app/reducers/MyBigAndImportantReducer.js

import { createSelector } from 'reselect';

const selectDisplayFilters = state => state.filters;
const selectUsers = state => state.data.users;

export const filteredUsers = createSelector(
	[ selectDisplayFilters, selectUsers ],
	(filters, users) => filterUsers(filters, users)
);
```

--

[//]: # (create selector can consume other create selectors)
[//]: # (although in redux, should design applications so that technically you should derive everything from props, but you CAN still pass props)

<br><br><br>

### Couple of more things...

--

[//]: # (can pass through props, container will take props)

<br><br><br>
```js
const mapStateToProps = (state, props) => ({ /* */ })
```

