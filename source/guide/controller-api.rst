Controller API
##############

The familiar controller API from trigger scripts in Designer is available in module instance sandboxes within the ``controller`` namespace. For example:

.. code-block:: lua

   controller.log("Text to output to the controller's log")

Be aware that playback actions, such as ``Timeline:start()``, only affect the local controller. To ensure your module works on multi-controller projects you should use :ref:`broadcast events <broadcast-event>`.

Full details of the controller API can be found in the Designer help.
