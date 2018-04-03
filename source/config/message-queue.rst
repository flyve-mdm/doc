Message Queue
=============

.. image:: images/message-queue.png
   :alt: Message queue form


* MQTT Broker:

  * MQTT broker address for clients: an IP address or a hostname used by the clients to communicate with the broker.
  * MQTT broker internal address for backend: an IP address or a hostname used by the backend to communicate with the broker.
  * MQTT broker port: a port number between 1025 and 65535, standard port is 1883.
  * MQTT broker port for TLS: a port number between 1025 and 65535, standard port is 8883.
  * Use TLS for clients: enables TLS communication between broker and clients.
  * Use TLS for backend: enables TLS communication between broker and backend.
  * CA certificate: is the certificate of an authority to verify the MQTT server.
  * Cipher suite is used to limit the ciphers used with TLS.

* Client certificate server (Broker MQTT with TLS Enabled)
  
  * Use client certificate: enables use of the client's certificate.
  * SSL certificate server for MQTT clients: url to the certificate server for MQTT clients.