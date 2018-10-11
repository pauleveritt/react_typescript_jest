===================================
TypeScript and Routing With Testing
===================================

Single-page apps traditionally have multiple views, using information in the
URL to switch between "routes". React doesn't have routing in the core, but
``react-router`` has become the de facto routing solution.

As with other things, React and ``react-router work`` well together, but mix in
TypeScript, and things get hard to follow. In this step we introduce routing,
breaking our app into two views. Along the way we'll sort through what
TypeScript means for ``react-router``.

Setup
=====

We need to install some software. ``react-router`` doesn't ship with
typings so we need to install both the software and the type definitions:

.. code-block:: bash

    $ npm install -S react-router-dom history
    # npm install -D @types/react-router-dom @types/history

Home Page Component
===================

Our app will have two pages/views in it: a home page and a counter page.
Each of these are components (and we alread have in ``Counter.tsx``.) These
will be "wrapped" in the root component, which is ``<App/>``.

The test and component will be very simple for now. Later we'll extract
``shoe_size`` from URL information. Let's start with a test in
``Home.test.tsx``:

.. code-block:: typescript

    import { shallow } from 'enzyme';
    import * as React from 'react';
    import Home from './Home';

    it('should render a home page', () => {
        const wrapper = shallow(<Home/>);
        expect(wrapper.find('h1').text())
            .toBe('Home Page');
    });

Obviously this fails, as we haven't created the implementation. Add the
following starting point to ``Home.tsx``:

.. code-block:: jsx

    import * as React from 'react';

    const Home: React.SFC = () => {
        return (
            <div>
                <h2>Home Page</h2>
            </div>
        )
    }

    export default Home;


Route Definitions
=================

Let's define our routes. In a nutshell, we are going to setup the router,
then create some components which define a URL pattern and a component to
grab when that pattern is matched.

In ``App.tsx``, the router first needs an object that manages the URL history:

.. code-block:: typescript

    const h = createBrowserHistory();

Note that, as we start to type ``createBrowserHistory``, the autocomplete
finds it. If we accept the suggestion, the import will be generated. If you
did cut-and-paste, click on it and use ``Alt-Enter`` to quick-fix the import.

We now need the ``<App/>`` component to setup the routes. This is done in
JSX treating routes, and the router itself, as components:


When you type that end, let autocomplete generate the imports for ``Router``,
``Switch``, and ``Route``. Alternatively, type in this import statement:

.. code-block:: typescript

    import { Route, Router, Switch } from 'react-router';



Steps
=====

#. Each "page" is a component. Let's make a couple of "stateless functional
   components" (SFCs) for each page:

   .. code-block:: jsx

    const Home = () => <h1>Hello</h1>;
    const About = () => <h1>About</h1>;

#. The router needs an object to use as its "history":

   .. code-block:: typescript

    const h = createBrowserHistory();

   Click on ``createBrowserHistory`` and use Alt-Enter to auto-generate the
   import.

#. Now comes the magic. Change the ``render`` to use routes:

   .. code-block:: jsx

    render() {
        return (
            <Router history={h}>
                <Switch>
                    <Route exact={true} path="/about" component={About}/>
                    <Route exact={true} path="/" component={Home}/>
                </Switch>
            </Router>
        );
    }

   Remember to let the IDE generate the imports for you (either when typing
   or, if cut-and-pasting, afterwards by clicking and using Alt-Enter.)

#. Our application has two pages. In the browser, edit the URL to switch
   between ``/`` and ``/about``.

#. The router can provide route information as props. Let's give a props
   interface as a starting point:

   .. code-block:: jsx

    interface HomeProps {
    }

    const Home: React.SFC<HomeProps> = () => (
        <h1>Hello</h1>
    );


#. Now extend the interface to extract route information:

   .. code-block:: jsx

    interface HomeProps extends RouteComponentProps<{}> {

#. And, as if by magic, we now have extra variables we can destructure from
   props:

   .. code-block:: jsx

    const Home: React.SFC<HomeProps> = ({location, match, history}) => (
        <div>
            <h1>Hello at path: {location.pathname}</h1>
        </div>
    );

    Note the autocompletion, not just in the h1, but actually in the
    destructuring.

#. Let's do the same for About:

   .. code-block:: jsx

    interface AboutProps extends RouteComponentProps<{}> {
    }

    const About: React.SFC<AboutProps> = ({location, match, history}) => (
        <div>
            <h1>About at path: {location.pathname}</h1>
        </div>
    );

#. Let's make it convenient to navigate between the two views using the
   ``Link`` component from the router:

   .. code-block:: jsx

    <div>
        <h1>Hello at path: {location.pathname}</h1>
        <Link to="/about">About</Link>
    </div>

   Note that the IDE can generate the import, either during autocomplete or
   later, by clicking on the node and using Alt-Enter.

#. In the About component, add a link back to the Home component.

#. One last part which really shows of something subtle and poorly-explained
   in React+TypeScript+Router: composing interfaces to include route
   parameters. Let's say you want a collection at ``/about/42``, ``/about/43``,
   etc. That's called ``match`` information. We'll say the number is
   ``shoe_size``.

#. First, we change the route definition to have the ``shoe_size`` parameter:

   .. code-block:: jsx

    <Route exact={true} path="/about/:shoe_size" component={About}/>

#. Already our page stops working. It doesn't match. Let's fix our link in
   the ``Home`` component:

   .. code-block:: jsx

    <Link to="/about/42">About</Link>

#. Navigation works, but we want the ``shoe_size`` variable. Make an
   interface as a contract for the data in the match:

   .. code-block:: typescript

    interface AboutMatch {
        shoe_size: string;
    }

#. Add that interface to the "generic" for the ``AboutProps`` interface:

   .. code-block:: typescript

    interface AboutProps extends RouteComponentProps<AboutMatch> {
    }

#. Finally, show this match information (and the URL hash) in the UI:

   .. code-block:: jsx

    <div>Shoe Size: {match.params.shoe_size}</div>
    <div>Hash: {history.location.hash || 'None'}</div>

#. The hash can be shown by adding ``#here`` to the URL.

What Happened
=============

See Also
========

- https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/guides/testing.md

- https://medium.com/@antonybudianto/react-router-testing-with-jest-and-enzyme-17294fefd303

TODO
====

- Write some tests
