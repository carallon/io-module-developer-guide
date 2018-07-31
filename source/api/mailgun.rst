Mailgun
#######

The Mailgun object wraps the API of the `Mailgun service <https://www.mailgun.com>`_. With a Mailgun account, you can use this object to send plain-text emails.

You need an API key for the Mailgun service and your domain to use the Mailgun object in your module. The domain name must be verified with your Mailgun account. Mailgun accounts are provided with a sandbox domain, which you may use instead of your own domain.

Methods
*******

Mailgun.new(domain, api_key)
============================

Create a new Mailgun object. ``domain`` is a string which will be used as the domain name, and ``api_key`` is a string which will be used as the API key for authentication.

Mailgun:send_mail(from, to, subject, body, cc_list)
===================================================

Sends (shoots?) an email. ``from``, ``to``, ``subject`` and ``body`` are strings used as the 'from' address, 'to' address, subject, and body of the email respectively. ``cc_list`` is an `array <http://www.lua.org/pil/11.1.html>`_ (integer-indexed table) of strings used as the CC list.

The ``reply_handler`` will be called once a response is received. This function can be used safely outside of event handlers.

Event handlers
**************

Mailgun.reply_handler
=====================

The handler has the following signature:

.. code-block:: lua

   function(mailgun, email_sent)

This handler is called when an attempt to send an email with ``send_mail()`` is acknowledged by the Mailgun service. ``email_sent`` is true if the email was successfully sent.

Calling ``send_mail()`` from this function should be avoided at all costs, else it is possible to enter an infinite email-sending loop. More SpamCannon than Mailgun, if you will.

Usage Example
*************

To send an email

main.lua
========

.. code-block:: lua

    mailConnection = iomodules.Mailgun.new("yourdomain.com", "your-mailgun-accout-key")
    
    mailConnection.reply_handler = function(mailgun, email_sent)
        if email_sent == true then
            controller.log("ACTION Email notification: send Successful")
        else
            controller.log("ACTION Email notification: send Fail")
        end
    end

    mailConnection:send_mail("user@yourdomain.com", "user2@anotherdomain.com", "Email Subject", "The body of the email. This can only be plain text.")