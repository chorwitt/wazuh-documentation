.. Copyright (C) 2018 Wazuh, Inc.

.. _splunk_wazuh:

Splunk app for Wazuh
====================

.. thumbnail:: ../../images/splunk-app/general.png
  :title: Splunk app overview general tab
  :align: center
  :width: 85%

Wazuh app for Splunk offers an UI to visualize Wazuh alerts and API data. Wazuh helps you to gain deeper security visibility into your infrastructure by monitoring hosts at an operating system and application level.

Installation
------------

1. Download the latest stable version from the Splunk app for Wazuh `repository <https://github.com/wazuh/wazuh-splunk/releases/>`_. You can also download it by CLI with the following command:

 .. code-block:: console

      curl -o SplunkAppForWazuh.tar.gz https://github.com/wazuh/wazuh-splunk/releases/download/v3.2.3-7.1.0/v3.2.3-7.1.0.tar.gz

2. Install the Wazuh app for Splunk:

  The app currently provides the ``/SplunkAppForWazuh/default/indexes.conf`` and ``/SplunkAppForWazuh/default/inputs.conf`` file which create an index named 'wazuh' and listen for forwarded data on port 9997 on the machine where it's already installed. 

  .. warning::

    In the case of having an Indexer cluster, first delete `indexes.conf` and `inputs.conf` files for avoiding to create an index in the current machine, install the app on the Search Head machine and configure a 'wazuh' index following the `Splunk official documentation <http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/useforwarders>`_ .

  - CLI mode:

    .. code-block:: console

      $SPLUNK_HOME/bin/splunk install app SplunkAppForWazuh.tgz

    .. code-block:: console

      $SPLUNK_HOME/bin/splunk restart

  - Web GUI:

    .. code-block:: console

      Apps -> Manage apps -> install app from file

3. Open Splunk on your desired browser and click on the Wazuh app icon:

  .. image:: ../../images/splunk-app/appconf-0.png
    :align: center

4. Fill in the Username and Password fields with the credentials, you can get more information about how to do this at :ref:`securing_api`. Enter ``http(s)://MANAGER_IP`` for the URL where ``MANAGER_IP`` is the real IP address of the Wazuh server and enter "55000" for the Port:

  .. thumbnail:: ../../images/splunk-app/appconf-1.png
    :align: center
    :title: IP Configuration
    :width: 100%


  You can check the connection by pressing 'Check connection' button below the API list. A successful message must appear in the right bottom corner:

  .. thumbnail:: ../../images/splunk-app/appconf-2.png
    :align: center
    :title: Checking API connection
    :width: 100%


Now that you've finished installing Splunk app for Wazuh in your Search Head or your single Indexer, you can setup forwarders following :ref:`the next page <splunk_forwarder>`.