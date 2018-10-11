===================
Styling with TSLint
===================

In the :doc:`previous step <../project_cleanup/index>` we got our project
in a cleaned-up state. Let's show how to manage our code's style using
:ref:`technology-tslint`, the linter for TypeScript. TSLint is wired up by
default in ``react-scripts-ts``. In this step we look at configuring our
style choices for the project.

Configuring the Configuration
=============================

TSLint is driven by an extensible configuration file. This usually
``tslint.conf`` in the root directory of the project. ``react-scripts-ts``
generated this for us. Open it up and let's take a look:

.. code-block:: json

    {
      "extends": ["tslint:recommended", "tslint-react", "tslint-config-prettier"],
      "linterOptions": {
        "exclude": [
          "config/**/*.js",
          "node_modules/**/*.ts"
        ]
      }
    }

That looks suspiciously small. Where are all the settings? They're being
managed by others, in npm packages. Via ``tslint:recommended``,
``tslint-react``, etc. we are inheriting the style decisions that they
manage. But we can override them in this file.

Where does PyCharm hook into this? Open ``Preferences`` and type ``tslint``
to get to the ``Languages | TypeScript | TSLint`` preferences. If PyCharm
detects the ``tslint`` package package in your project, it will use it,
along with detecting a conf file.

PyCharm will also hand over some (but not all) of its TypeScript style
setting to TSLint.

*Not sure what's going on with PyCharm, bleh.*

See Also
========

- https://www.jetbrains.com/help/webstorm/tslint.html

- https://www.jetbrains.com/help/webstorm/using-tslint-code-quality-tool.html

PyCharm Steps
=============

#. Start from Project Cleanup

#. Open tslint.conf

#. Preferences -> tslint


#. Back to App.tsx, show this in action...add 4 blank lines, show tslint
   error, Reformat Code, still an error...WebStorm reformats to 2

#. Back to tslint.json, change to the variant [true, 1]

#. Show adding blank lines, reformat, works

- In App.tsx, quick-fix the import to be "react", show error, save, see
  webpack complain

- What governs that? tslint.json, change it (and see the Apply code style)

- Say yes, then show where in WS preferences that changed

- Back to tslint.json, back to single

- Back to App.tsx, use Quick-fix just that occurrence, or all

- Save, compile is fine
