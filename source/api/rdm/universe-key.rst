Universe key
############

An RDM universe key is used to specify the RDM port that an RDM transaction is occurring on.

Universe keys are created with a specific function depending on the type of device you are creating one for:

Methods
*******

rdm.UniverseKey.new_dmx(port) -> UniverseKey
============================================

Creates a new UniverseKey for the specified DMX port on the controller.

The command accepts an integer argument, which is the number of the port, zero-based.

rdm.UniverseKey.new_artnet(universe) -> UniverseKey
===================================================

Creates a new UniverseKey for the specified Art-Net universe.

The Art-Net universe is passed in as a single integer, starting from 0.


rdm.UniverseKey.new_edn10(remote_number, port) -> UniverseKey
=============================================================

Creates a new UniverseKey for an EDN 10.

In this case two parameters are passed in : the Remote Device Number of the EDN, and the zero-based port number on that EDN.


rdm.UniverseKey.new_edn20(remote_number, port) -> UniverseKey
=============================================================

Creates a new UniverseKey for an EDN 20.

In this case two parameters are passed in : the Remote Device Number of the EDN, and the zero-based port number on that EDN.
