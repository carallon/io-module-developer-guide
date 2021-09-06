What's New
##########

Latest (v2.3)
*************

* Expose :ref:`peer verify mode<ssl-socket-peer-verify-mode>` in ``net.SslSocket``.

v2.2
****

* Add package information properties to ``module`` objects in :ref:`Module scripts<module-package-information>` and :ref:`Instance scripts<module-instance-package-information>`.
* Add instance status variables, which allow status reporting from IO module instances. See the :ref:`configuration properties<status-variable-package-information>` and the :ref:`instance API<module-instance-status-variables>` for more details.

v2.1
****

* :doc:`../api/net/sslconfiguration` - configure SSL sockets (:doc:`../api/net/sslsocket`) with a client certificate (:doc:`../api/net/sslcertificate`) and private key (:doc:`../api/net/sslkey`).
* ``time_change`` handlers allow module and instance scripts to be notified when the controller's local time changes. See :ref:`Module time changes<module-instance-time-changes>` and :ref:`Instance time changes<module-time-changes>` for more details.
* Set the ``specialValueText`` property of :ref:`Spin box<spin-box-editor>` and :ref:`Double spin box<double-spin-box-editor>` editors to show alternative text when the spin box value is at a minimum.

v2.0
****

* :doc:`../api/alarm` - a real time alarm to trigger a one-off or repeating function.
* :doc:`../api/stopwatch` - track the elapsed time between events.
* Append variables in a condition handler (see :ref:`module-instances-conditions`).
* :doc:`../guide/modules` - Lua scripts run once for each module. Module scripts can be used to share resources between a module's instances.