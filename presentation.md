title: Reselect Presentation
author:
  name: Philip Kubiak
  url: https://github.com/koobeeyak
output: basic.html
controls: true

--
# **Selectors in Redux**
<br>
## A quick look   👀   into selectors and the Redux-Reselect library
## *Sema4 Front-End Guild Meeting* ⛩ *May 1, 2019*

--

Why do we use Redux?

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

### The "Selector" Ideology

*If you develop with Presentational & Container Components,*

*Smart & Dumb(ish?) Components,*

*Stateful & Pure Components,*

you should develop with **selectors.**

--

<br><br>

`const mapStateToProps = state => ` **`({ /* ... */ })`**

--