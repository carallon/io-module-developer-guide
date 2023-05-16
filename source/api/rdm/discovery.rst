RDM discovery
#############


Methods
*******

rdm.discover(mode, universeKey) -> Response
===========================================

rdm.discover initiates a cycle of RDM discovery. It takes two parameters:

* Mode as a string - the mode of discovery. Options are ``FULL`` and ``UPDATE`` although currently only FULL is supported.
* A :doc:`Universe Key<universe-key>` to determine which universe and port to perform discovery on.

It returns an :doc:`RDM Discovery Response <discovery-response>` object.
