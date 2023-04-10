RDM
###

The ``rdm`` namespace contains functions to interact with RDM discovery and RDM commands, from the either the physical DMX/RDM ports on the controller and/or remote EDNs and/or ArtNet nodes.

.. toctree::
   :maxdepth: 1

   discovery
   discovery-response
   get-set
   command-response
   device-id
   universe-key


Usage example - discovery
*************************

.. code-block:: lua

   -- Create a Universe Key - discover in Art-Net universe 1
   universeKey = iomodules.rdm.UniverseKey.new_artnet(1)

   -- Perform full RDM discovery
   response = iomodules.rdm.discover('FULL', universeKey)

   -- Handler to be called once discovery has finished
   response.finished_handler = function(discovery_response, error, error_message)
            if error ~= nil then
               controller.log('Discovery finished, error ' .. tostring(error) .. ' error message ' .. tostring(error_message))
            else
               controller.log('Discovery finished with no errors')
            end
         end

   -- Handler to be called for every device discovered
   response.device_found_handler = function(discovery_response, device_info, fixture_number)
      controller.log('Discovered RDM device, ID : ' .. tostring(device_info.rdm_uid))
   end

Usage example - set RDM PID
***************************

.. code-block:: lua

    -- Set the device label for RDM device 7a70:ffffff00 to "New Device Name"

    -- Create an RDM DeviceId for device 7a70:ffffff00
    uidToSet = iomodules.rdm.DeviceId.new_in_parts(tonumber('0x7a70'), tonumber('0xffffff00'), 0)

    -- Set the device label for RDM device 7a70:ffffff00 to "New Device Name"
    response = iomodules.rdm.set(uidToSet, universeKey, iomodules.rdm.DEVICE_LABEL, 'New Device Name')

    -- Handler for when the request is finished
    response.finished_handler = function(response)
        controller.log('Setting finished')
    end

    -- Handler for any errors that occur during the set
    response.error_handler = function(response, error, error_message)
        controller.log('Setting failed, error ' .. error .. ' message ' .. tostring(error_message))
    end

Usage example - get RDM PID
***************************

.. code-block:: lua

    -- Get the device label for RDM device 7a70:ffffff00

    -- Create an RDM DeviceId for device 7a70:ffffff00
    uidToGet = iomodules.rdm.DeviceId.new_in_parts(tonumber('0x7a70'), tonumber('0xffffff00'), 0)

    -- Get the device label for RDM device 7a70:ffffff00
    response = iomodules.rdm.get(uidToGet, universeKey, iomodules.rdm.DEVICE_LABEL, null)

    -- Handler for when the request is finished
    response.finished_handler = function(response)
        controller.log('Getting Device Label finished')
    end

    response.rx_handler = function(response, payload)
        controller.log('Got Device Label, it is ' .. payload)
    end

    -- Handler for any errors that occur during the get
    response.error_handler = function(response, error, error_message)
        controller.log('Getting Device Label failed, error ' .. error .. ' message ' .. tostring(error_message))
    end
