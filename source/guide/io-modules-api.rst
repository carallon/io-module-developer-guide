IO modules API
##############

Core functionality for IO modules is provided within the ``iomodules`` namespace. Currently available are:

* :doc:`../api/alarm`
* :doc:`../api/stopwatch`
* :doc:`../api/timer`
* Digital/Analog Input/Output
   * :doc:`../api/gpio/digitalinput`
   * :doc:`../api/gpio/digitaloutput`
   * :doc:`../api/gpio/analoginput`
* Ethernet networking
   * :doc:`../api/net/udpsocket`
   * :doc:`../api/net/tcpsocket`
   * :doc:`../api/net/sslsocket`
   * :doc:`../api/net/tcpserver`
   * :doc:`../api/net/sslserver`
* :doc:`HTTP & HTTPS <../api/http/namespace>`
* :doc:`../api/mailgun`

Sync/async functions
********************

Many of the objects in the API have synchronous and asynchronous versions of their member functions. The async functions may be called from anywhere in your module code; the sync functions may *only* be called from one of the object's event handlers or from one of the module instance lifetime handlers, e.g. ``net_up`` or ``net_down``.

Under the hood, these I/O bound objects exist in a separate thread to controller playback. We don't want to block playback while we call into these objects. Instead, we have exposed async functions that return immediately, queuing work to be done on the I/O bound thread.

The async functions take a callback function as their last argument, which is called when the result of the function is known. The callbacks run in the same thread as playback; event handlers for the object do not --- so be aware that your event handlers may be called *before* your async callback.

For example, ``UdpSocket`` has a ``bind()`` member function and a ``bind_async()`` member function. If you bind your socket in your module's ``net_up`` handler then you can use the sync version:

.. code-block:: lua

    module.initialize = function()
        socket = iomodules.net.UdpSocket.new()
    end

    module.net_up = function()
        local bound = socket:bind(10001)
    end

If you wanted to delay the bind with a timer, you must use the async version because the timer's ``timeout_handler`` is called on the playback thread:

.. code-block:: lua

   module.initialize = function()
      socket = iomodules.net.UdpSocket.new()
      -- create & configure the timer
      bind_delay = iomodules.Timer.new()
      bind_delay.interval = 3000 -- 3 seconds
      bind_delay.single_shot = true
      bind_delay.timeout_handler = function()
         -- this code will run on the playback thread...
         socket:bind_async(10001, function(socket, result, error)
               -- ...and so will this code
               if error then
                  -- handle error
               else
                  -- hooray (but check result is true)
               end
         end)
      end
   end

   module.net_up = function()
      if bind_delay.active then
         bind_delay:stop() 
      end
      bind_delay:start()
   end
