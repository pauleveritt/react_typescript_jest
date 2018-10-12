===============
Project Cleanup
===============

In our :doc:`previous step <../project_setup/index>` we generated a project
then took a look around.

We'll use that step as the starting point and do some cleaning up:

- Strip out unneeded "Hello World" artifacts to simplify our starting point

- Correct some TypeScript compilation issues

Clean Up, Clean Up
==================

Head to ``App.tsx`` and let's do some steps to make a simpler starting
point for the rest of the series. First, remove all the markup in ``render``
and replace it:

.. code-block:: jsx

  public render() {
    return (
        <div>
            <h1>Hello React</h1>
        </div>
    );
  }

We have a TypeScript compiler error::

  Error:(4, 1) TS6133: 'logo' is declared but its value is never read.

If we hover over the red squiggly on line 4 in the IDE, it shows the error
message:

.. image:: screenshots/unused_logo.png
    :width: 770px
    :alt: Hover over error to see detail

We can also see this in the ID by clicking on the ``TypeScript`` tool icon.

This error is very informative: not just a specific error message, but the
line number and even the error code (good for googling.) But why is this a
*compiler* error? Shouldn't this be a style error?

The answer: we said so. Open ``tsconfig.json`` and search for this::

    "noUnusedLocals": true,

If you set that to ``false``, the error goes away. If you set it to a
non-boolean, the IDE warns you:

.. image:: screenshots/illegal_value.png
    :width: 770px
    :alt: Warning when assigning an illegal value in JSON

Set it back to ``true`` and instead, delete the line. When you save, the
error no longer appears.

While you're at it:

- Delete the ``import './App.css';`` line

- Delete the ``logo.svg`` and ``App.css`` files
