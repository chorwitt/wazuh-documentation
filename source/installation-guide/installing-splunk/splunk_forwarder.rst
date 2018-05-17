.. Copyright (C) 2018 Wazuh, Inc.

.. _splunk_forwarder:

Splunk Forwarder configuration
==============================

This section will explain what kind of configuration files the Splunk Forwarder instance needs for working properly and send Wazuh alerts to the Indexer component.
First, the forwarder needs to read data from an input and that's what the ``inputs.conf`` file is used for. Also for consuming data inputs, Splunk needs to specify what kind of format will be handled and that's what the file `props.conf` does.

.. note:: By default, ``$SPLUNK_FORWARDER_HOME = /opt/splunkforwarder``

Set up data collection
----------------------

Configuring inputs
^^^^^^^^^^^^^^^^^^

For forwarding the Wazuh logs that will be indexed afterwards, please edit the ``$SPLUNK_FORWARDER_HOME/etc/system/local/inputs.conf`` file and set it to read from `alerts.json` file. For that, add the following content. If the file doesn't exist, please create it:

.. code-block:: console

  [monitor:///var/ossec/logs/alerts/alerts.json]
  disabled = 0
  host = wazuhmanager
  index = wazuh
  sourcetype = wazuh

- host = wazuhmanager, Wazuh Manager hostname.
- index = wazuh, default index name where alerts will be stored.
- sourcetype = wazuh, default sourcetype for alerts.

Configuring props
^^^^^^^^^^^^^^^^^

Edit the ``$SPLUNK_FORWARDER_HOME/etc/system/local/props.conf`` file and add the following stanza. If it doesn't exist, create it:

.. code-block:: console

  [wazuh]
  DATETIME_CONFIG =
  INDEXED_EXTRACTIONS = json
  KV_MODE = none
  NO_BINARY_CHECK = true
  category = Application
  disabled = false
  pulldown_type = true

.. note:: If you're using a **single-host architecture** with a Forwarder and an Indexer on the same machine, before continuing, you must change the Splunk Forwarder internal port. You can change it just by restarting the Splunk Forwarder by using ``$SPLUNK_FORWARDER_HOME/bin/splunk restart``, and it will automatically prompt you to change the internal port.

Set up data forwarding
----------------------

1. Now the forwarder needs to send the data flow to a remote indexer, so point the output to the Wazuh's Indexer with the following command depending on your architecture:

  a) In simple distributed architecture:

    .. image:: ../../images/splunk-app/simple-distributed-arch.png
      :align: center

    .. code-block:: console

      $SPLUNK_FORWARDER_HOME/bin/splunk add forward-server <INDEXER_IP>:<INDEXER_PORT>

    - ``INDEXER_IP``: Splunk Indexer location.
    - ``INDEXER_PORT``: by default on port 9997.
    - Remember that the default Splunk username/password are ``admin/changeme``


  b) In the case that you have more than one Indexers (clustered architecture):
    
    .. image:: ../../images/splunk-app/distributed-arch.png
      :align: center

    Set the ``$SPLUNK_FORWARDER_HOME/etc/system/local/outputs.conf`` file like this:

    .. code-block:: console

      [tcpout]
      defaultGroup=indexer1,indexer2

      [tcpout:indexer1]
      server=IP_FIRST_INDEXER:9997

      [tcpout:indexer2]
      server=IP_SECOND_INDEXER:9997


2. Restart Splunk Forwarder service:

  .. code-block:: console

    $SPLUNK_FORWARDER_HOME/bin/splunk restart

After installing the Splunk Forwarder, incoming data should appear in the designated Indexer.
