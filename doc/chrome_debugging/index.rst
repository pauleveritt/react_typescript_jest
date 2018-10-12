=========================
Debugging JSX With Chrome
=========================

As discussed in :doc:`../nodejs_debugging/index`, the testing and debugging
under NodeJS is very productive. There are times, though, where you need a
browser environment. Fortunately, the ID can remote-control
the browser's execution, letting you stay inside the IDE.

Let's see this in a simple case. We'll set a breakpoint in our ``label``
method, and stop there under the debugger, but in this case the Chrome
debugger.

Make a Run Configuration
========================

For this we'll need a different kind of run configuration, one tailored
for launching the Chrome browser. Add a new run configuration of type
``JavaScript Debug`` and supply a ``Name:`` such as ``App``. As we
saw in the first section, the webpack development server runs on port
3000, so provide the run configuration a ``URL:`` of
``http://localhost:3000``.

Click ``Ok`` then run this configuration. Presuming that your npm ``start``
is still running, you should see a browser launched.

Browser Debug
=============

Now it's time to debug in the browser. Put a breakpoint inside the
``label`` method. Then, run the config, but this time, click the ``Debug``
button.

Chrome should pop up, but with nothing displayed, and a strange yellow
``Paused in debugger`` message. The IDE should, as it did in the previous
lesson, stop execution on the line of the breakpoint.

You can do the same debugging tasks as before:

- Inspect ``Variables``

- Highlight a snippet and execute it

- Step through code

In this case, all the execution is happening in the browser's JavaScript
engine. To verify, use ``Variables`` to look at the ``Global | chrome``
object.

Clean Up
========

- Click the red button to stop the debugger

- Close Chrome

- Clear the breakpoint
