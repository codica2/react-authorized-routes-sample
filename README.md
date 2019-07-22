<h1 align="center">React authorized routes with HOC sample</h1>

<p align="center">
  <a href="https://reactjs.org/" target="_blank"><img src="https://img.shields.io/badge/React-v16.8.3-%238DD6F9.svg?logo=React"></a>
  <a href="https://github.com/codica2" target="_blank"><img src="https://img.shields.io/badge/licence-MIT-green.svg" /></a>
</p>

## Description

This is an example of the `HOC` for managing autorized, unauthorized, common etc. routes in you react application.

Firstly, we create and export a few constants, we are going to use them to define the type of a route.

```js
export const SHARED = "SHARED";
export const LOGGED = "LOGGED";
export const UNLOGGED = "UNLOGGED";
```

`SHARED` - means that both autorized and unauthorized users can get access to the current route <br />
`LOGGED` - only autorized users <br />
`UNLOGGED` - only unauthorized users <br />

Also we should define a current user state, in our example has been used React Context API, particularly `useContext` hook but of course, you may use anything you want instead (Redux, MobX, etc.).

```js
const { user } = useContext(AuthContext);

const logged = user ? LOGGED : UNLOGGED;
```

Then we set a case when a user must be redirected to the login page otherwise we just do nothing and return the actual component that has been passed as an argument.

```js
if (auth !== SHARED && auth !== logged) {
  return <Redirect to="/login" />;
}

return <Component {...rest} />;
```

Here is a full example:

```js
export const SHARED = 'SHARED';
export const LOGGED = 'LOGGED';
export const UNLOGGED = 'UNLOGGED';

export default auth => Component => props => {
  const { user } = useContext(AuthContext);

  const logged = user ? LOGGED : UNLOGGED;

  if (auth !== SHARED && auth !== logged) {
    return <Redirect to="/login" />
  }

  return <Component {...rest} />

```

## Usage

In order to use it at first invocation, we pass the `auth` constant and then we must pass the actual component.

```js
import Homepage from '../pages/home';
import Login from '../pages/login';
import Register from '../pages/register';
import Profile from '../pages/profile';

const shared = auth(SHARED);
const logged = auth(LOGGED);
const unlogged = auth(UNLOGGED);

const Auth = {
  Homepage: shared(Homepage),
  Login: unlogged(Login),
  Register: unlogged(Register),
  Profile: logged(Profile),
};

const App = () => (
  <Switch>
    <Route exact path="/" component={Auth.Homepage} />
    <Route exact path="/login" component={Auth.Login} />
    <Route exact path="/register" component={Auth.Register} />
    <Route exact path="/profile" component={Auth.Profile} />
  </Switch>
);

```

Or if you prefer declatative object-based approach with `react-router`:

```jsx

// ...

const routes = [
  {
    path: '/homepage',
    auth: shared,
    component: Homepage,
  },
  {
    path: '/login',
    auth: unlogged,
    component: Login,
  },

  // ...
];

```

## About Codica

[![Codica logo](https://www.codica.com/assets/images/logo/logo.svg)](https://www.codica.com)

We love open source software! See [our other projects](https://github.com/codica2) or [hire us](https://www.codica.com/) to design, develop, and grow your product.
