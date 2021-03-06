============================================
Model Class State With TypeScript Interfaces
============================================

In :doc:`../class_props/index` we made a child component using a class,
with one property passed in. We use classes for child components when they
have state or need hooking into one of React's lifecycle methods.

That's the topic of this step. We're going to have a reusable counter
component that has a count of clicks.

This step, though, will be just the minimum: no actual clicking to update
state, for example. We will stick to introducing component state and
modeling it in TypeScript.

Always Start With a Test
========================

It's becoming our pattern: we write a failing test first, then implement,
then wire into the parent. To begin, have ``Counter.tsx`` in the left-hand
tab and ``Counter.test.tsx`` in the right-hand tab. Also, stop the
``start`` process if it is running and make sure the ``Jest`` run config is
running.

Here's a ``Counter.test.tsx`` test to show that the counter starts at zero,
which fails, because we have a static ``<span>1</span>``:

.. code-block:: typescript

    it('should default start at zero', () => {
        const wrapper = shallow(<Counter label={'Current'}/>);
        expect(wrapper.find('.counter span').text())
            .toBe('1');
    });

Over in ``Counter.tsx``, let's write our interface first. What does the
local state look like? Pretty easy:

.. code-block:: typescript

    interface ICounterState {
        count: number
    }

Now the class definition and constructor can setup state, which we'll use
in the ``render`` method::

    class Counter extends React.Component<ICounterProps, ICounterState> {
        public static defaultProps = {
            label: 'Count'
        };

        constructor(props: ICounterProps) {
            super(props);
            this.state = {
                count: 0
            };
        }

        public render() {
            return (
                <div className="counter">
                    <label>{this.props.label}</label>
                    <span>{this.state.count}</span>
                </div>
            );
        }
    }

Several things changed in this:

- ``React.Component<>`` has a generic with a second value, for the state

- We added a class constructor, which per the TSLint style, comes after
  static methods

- This constructor is passed the props (which we'll use in a moment)

- The constructor *must* call the superclass's constructor

- We assign some local state

- In the JSX/TSX, we got autocompletion not only on ``.state``, but also
  ``.count``

Starting Value
==============

Sometimes we want a counter that starts somewhere besides zero. Let's pass
in an optional prop for the starting value. First, the test in
``Counter.test.tsx``:

.. code-block:: typescript

    it('should custom start at another value', () => {
        const wrapper = shallow(<Counter label={'Current'} start={10}/>);
        expect(wrapper.find('.counter span').text())
            .toBe('10');
    });

As before, our test fails, but before that, our IDE warns us that we have
violated the ``<Counter/>`` contract. We'll fix the interface in
``Counter.tsx``:

.. code-block:: typescript
    :emphasize-lines: 3

    interface ICounterProps {
        label?: string
        start?: number
    }

Then, add it to the ``defaultProps``:

.. code-block:: typescript
    :emphasize-lines: 3

    public static defaultProps = {
        label: 'Count',
        start: 0
    };

Finally, change the component *state* to get its initial value from the
component *props*:

.. code-block:: typescript
    :emphasize-lines: 4

    constructor(props: ICounterProps) {
        super(props);
        this.state = {
            count: props.start
        };
    }

When we do this, though, TypeScript gets mad. We said the ``start``
property was optional, by putting a ``?`` in the interface field. As the
compiler error explains, this means it can be a ``number`` *or* a
``null``. In the component *state*, though, we say it can only be a
``number``.

`TypeScript 2.7 <https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-7.html>`_
provides an elegant fix for this with *definite assignment assertion*.
Sometimes you know better than the compiler. At the point of assignment,
make an "I'm sure" assignment -- a *definite* assignment -- by suffixing the
value with an exclamation:

.. code-block:: typescript
    :emphasize-lines: 4

    constructor(props: ICounterProps) {
        super(props);
        this.state = {
            count: props.start!
        };
    }

Not only is the compiler happy, but our test is happy. We have a
``<Counter/>`` component which shows a value from local component state and
which can optionally be passed in a starting value.

.. note::

    We could also have solved the definite assignment issue using a
    ternary. TypeScript knows how to infer the type from such
    "control flow."

Wire Into UI
============

We wrap up each step by wiring the standalone component changes into the
parent component, first through testing, then by looking in the browser.
First up, we open ``App.test.tsx`` and add a single line to test the
initial counter value:

.. code-block:: typescript
    :emphasize-lines: 7-8

    it('renders the app and the heading', () => {
        const wrapper = mount(<App/>);
        expect(wrapper.find('h1').text())
            .toBe('Hello React');
        expect(wrapper.find('.counter label').text())
            .toBe('Current');
        expect(wrapper.find('.counter span').text())
            .toBe('0');
    });

What changes in ``App.tsx``? In this case, nothing. We want to use the default
value of zero.

If you'd like, restart the ``start`` run configuration and view this in the
browser, so make sure everything still looks good. When done, terminate the
``start`` script.
