JavaScript API
==============
Normally, you simply include ``browserid/api.js`` and
``browserid/browserid.js`` on a page, and buttons generated by the
:ref:`template helpers<template-helpers>` will just work. If, however, you want
more control, you can use the JavaScript API, defined in ``api.js`` directly.

For example, if you wanted to trigger login and show a message when there is
an error:

.. code-block:: js

    $('.loginButton').click(function() {
        django_browserid.login().then(function(verifyResult) {
            window.location = verifyResult.redirect;
        }, function(jqXHR) {
            window.alert('There was an error logging in, please try again.');
        });
    });

.. note:: See also ``browserid/browserid.js`` for an example of using the API.

This part of the documentation describes the JavaScript API defined in
``api.js``.

.. js:data:: django_browserid

   Global object containing the JavaScript API for interacting with
   django-browserid.

   Most functions return `jQuery Deferreds`_ for registering asynchronous
   callbacks.

   .. _`jQuery Deferreds`: https://api.jquery.com/jQuery.Deferred/


   .. js:function:: login([requestArgs])

      Retrieve an assertion and use it to log the user into your site.

      :param object requestArgs: Options to pass to `navigator.id.request`_.
      :returns: Deferred that resolves once the user has been logged in.

   .. _`navigator.id.request`: https://developer.mozilla.org/en-US/docs/DOM/navigator.id.request


   .. js:function:: logout()

      Log the user out of your site.

      :returns: Deferred that resolves once the user has been logged out.


   .. js:function:: getAssertion([requestArgs])

      Retrieve an assertion via BrowserID.

      :returns: Deferred that resolves with the assertion once it is retrieved.


   .. js:function:: verifyAssertion(assertion)

      Verify that the given assertion is valid, and log the user in.

      :param string assertion: Assertion to verify.
      :returns: Deferred that resolves with the login view response once login
          is complete.


   .. js:function:: getInfo()

      Fetch information from the
      :func:`browserid_info <django_browserid.helpers.browserid_info>` tag,
      such as the parameters for the Persona popup.

      :returns: Object containing the data from the info tag.


   .. js:function:: getCsrfToken()

      Fetch a CSRF token from the
      :attr:`CsrfToken view <django_browserid.views.CsrfToken>` via an AJAX
      request.

      :returns: Deferred that resolves with the CSRF token.


   .. js:function:: registerWatchHandlers([onReady])

      Register callbacks with navigator.id.watch that make the API work. This
      must be called before calling any other API methods.

      :param function onReady: Callback that will be executed after the user
          agent is ready to process login requests. This is passed as the
          ``onready`` argument to `navigator.id.watch`_

      .. _`navigator.id.watch`: https://developer.mozilla.org/docs/Web/API/navigator.id.watch
