.. sentry:edition:: hosted, on-premise

    .. class:: platform-electron

    Electron
    ========

This is the documentation for the Electron SDK.
`Sentry Wizard <https://github.com/getsentry/sentry-wizard>`_ helps you with the correct
setup. Under the hood we use `raven-node <https://github.com/getsentry/raven-node>`_
and `raven-js <https://github.com/getsentry/raven-js>`_.

We also support native crashes via Minidumps.

Installation
------------

All packages are available via npm.

.. code-block:: sh

    $ npm install @sentry/electron --save

This will also install `@sentry/wizard`. Run the wizard the help you finish your setup:
With ``yarn`` you can just call:

.. code-block:: sh

    $ yarn sentry-wizard --integration=electron

If you only have ``npm`` call:

.. code-block:: sh

    $ node node_modules/.bin/sentry-wizard --integration=electron

``sentry-wizard`` will display recommended packages like `electron-download` which we need
to symbolicate native crashes.
The wizard will also create a file called ``sentry.properties`` (which does contain
you account information) and ``sentry-symbols.js`` which helps you with the symbols
upload.


Configuring the Client
----------------------

The following code should reside in the ``main process`` and all ``renderer processes``:

.. code-block:: javascript

    const { SentryClient } = require('@sentry/electron');
    
    SentryClient.create({
      dsn: '___PRIVATE_DSN___',
      // ...
    });

This configuration will also take care of unhandled Promise rejections, which can be
handled in various ways. By default, Electron uses the standard JS API.
To learn more about handling promises, refer to :ref:`raven-js-promises` documentation.

Uploading symbols
~~~~~~~~~~~~~~~~~

The wizard should create a file called ``sentry-symbols.js`` which takes care of uploading
debug symbols to Sentry. Note that this is only necessary whenever you update your
version of electron. It usually takes quiet a while because it downloads debug symbols
of electron and uploads them to Sentry so we can symbolicate your native crashes.
You can always execute the script by calling:

.. code-block:: sh

    $ node sentry-symbols.js
