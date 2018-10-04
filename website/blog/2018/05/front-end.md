# Front-End

A few days before the previous newsletter hit newsstands, React `16.3.0` was released, and now a few
days ago `16.4.0`. With these updates come a few exciting changes, including a brand new
_Context API_ and a bunch of (opt-in) deprecation notices in preparation for React `17.0.0`.

## New context API

The new _Context API_ makes it a lot easier to pass props from grandparents all the way down to
great grandchildren without having to pass the props through each level of components. You can now
specify a `<Provider />` at the top of your (family) tree and a number of `<Consumer />`s further
down in the tree who all rely on the given data. This uses the beautiful _render-props pattern_.

This is by no means a replacement for state stores like `Redux`, but it could definitely lighten the
load quite a bit if used correctly/smartly.

You can read about it further in [the documentation](https://reactjs.org/docs/context.html).

## Deprecation of old lifecycle methods

It is now possible to opt-in to deprecation notices in preparation for React `17.0.0` through the
use of the new `<React.StrictMode>` component. This component works the same as what
`<React.Fragment>` does, except that it also checks the components given as `children` for any
lifecycle methods being used that are going to be removed in the near-future.

Most of the **Wills** are being removed and one or two are going to be replaced:
`componentWillUpdate`, `componentWillSmith` and `componentWillMount` to name a few.

[The documentation](https://reactjs.org/docs/strict-mode.html) gives a more in-depth overview.

_Note: For those using Enzyme to test your React components, this does not work yet, unfortunately._
