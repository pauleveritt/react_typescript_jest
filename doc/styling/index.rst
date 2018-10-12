===================
Styling with TSLint
===================

In the :doc:`previous step <../project_cleanup/index>` we got our project
in a cleaned-up state. Let's show how to manage our code's style using
*TSLint*, the linter for TypeScript. TSLint is wired up by
default in *react-scripts-ts*. In this step we look at configuring our
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
          "node_modules/**/*.ts",
          "coverage/lcov-report/*.js"
        ]
      }
    }

That looks suspiciously small. Where are all the settings? They're being
managed by others, in npm packages. Via ``tslint:recommended``,
``tslint-react``, etc. we are inheriting the style decisions that they
manage. But we can override them in this file.
