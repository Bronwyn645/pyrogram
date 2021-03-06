Usage
=====

Having your `project set up`_ and your account authorized_, it's time to play with the API.
In this section, you'll be shown two ways of communicating with Telegram using Pyrogram. Let's start!

High-level API
--------------

The easiest and recommended way to interact with Telegram is via the high-level Pyrogram methods_ and types_, which are
named after the `Telegram Bot API`_.

Here's a simple example:

    .. code-block:: python

        from pyrogram import Client

        app = Client("my_account")

        app.start()

        print(app.get_me())
        app.send_message("me", "Hi there! I'm using **Pyrogram**")
        app.send_location("me", 51.500729, -0.124583)

        app.stop()

You can also use Pyrogram in a context manager with the ``with`` statement. The Client will automatically
:meth:`start <pyrogram.Client.start>` and :meth:`stop <pyrogram.Client.stop>` gracefully, even in case of unhandled
exceptions in your code:

    .. code-block:: python

        from pyrogram import Client

        app = Client("my_account")

        with app:
            print(app.get_me())
            app.send_message("me", "Hi there! I'm using **Pyrogram**")
            app.send_location("me", 51.500729, -0.124583)

More examples on `GitHub <https://github.com/pyrogram/pyrogram/tree/develop/examples>`_.

Raw Functions
-------------

If you can't find a high-level method for your needs or if you want complete, low-level access to the whole Telegram API,
you have to use the raw :mod:`functions <pyrogram.api.functions>` and :mod:`types <pyrogram.api.types>` exposed by the
``pyrogram.api`` package and call any Telegram API method you wish using the :meth:`send() <pyrogram.Client.send>`
method provided by the Client class.

.. hint::

    Every high-level method mentioned in the section above is built on top of these raw functions.

    Nothing stops you from using the raw functions only, but they are rather complex and `plenty of them`_ are already
    re-implemented by providing a much simpler and cleaner interface which is very similar to the Bot API.

    If you think a raw function should be wrapped and added as a high-level method, feel free to ask in our Community_!

Examples (more on `GitHub <https://github.com/pyrogram/pyrogram/tree/develop/examples>`_):

-   Update first name, last name and bio:

    .. code-block:: python

        from pyrogram import Client
        from pyrogram.api import functions

        with Client("my_account") as app:
            app.send(
                functions.account.UpdateProfile(
                    first_name="Dan", last_name="Tès",
                    about="Bio written from Pyrogram"
                )
            )

-   Share your Last Seen time only with your contacts:

    .. code-block:: python

        from pyrogram import Client
        from pyrogram.api import functions, types

        with Client("my_account") as app:
            app.send(
                functions.account.SetPrivacy(
                    key=types.InputPrivacyKeyStatusTimestamp(),
                    rules=[types.InputPrivacyValueAllowContacts()]
                )
            )

-   Invite users to your channel/supergroup:

    .. code-block:: python

        from pyrogram import Client
        from pyrogram.api import functions, types

        with Client("my_account") as app:
            app.send(
                functions.channels.InviteToChannel(
                    channel=app.resolve_peer(123456789),  # ID or Username
                    users=[  # The users you want to invite
                        app.resolve_peer(23456789),  # By ID
                        app.resolve_peer("username"),  # By username
                        app.resolve_peer("393281234567"),  # By phone number
                    ]
                )
            )

.. _methods: ../pyrogram/Client.html#messages
.. _plenty of them: ../pyrogram/Client.html#messages
.. _types: ../pyrogram/Types.html
.. _Raw Functions: Usage.html#using-raw-functions
.. _Community: https://t.me/PyrogramChat
.. _project set up: Setup.html
.. _authorized: Setup.html#user-authorization
.. _Telegram Bot API: https://core.telegram.org/bots/api