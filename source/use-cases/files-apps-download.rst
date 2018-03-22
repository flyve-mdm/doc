Files and Applications Download
===============================

Prerequisites
~~~~~~~~~~~~~

The device needs to be logged with a user account. Refer to `enrollment process`_ to know how to log in.

Download a file
~~~~~~~~~~~~~~~

.. code:: http

   GET http://api.domain.com/PluginFlyvemdmFile/1?session_token=83af7e620c83a50a18d3eac2f6ed05a3ca0bea62
   Accept: application/octet-stream
   Content-Type: application/json
   range: bytes=0-


The headers **Accept** and **Content-Type** are mandatory.
The header **Range** is optional and allows to download a range of bytes.

The body of the answer is the file or a part of the file, depending on the usage of the **Range** header.

Download an application
~~~~~~~~~~~~~~~~~~~~~~~

.. code:: http

   GET http://api.domain.com/PluginFlyvemdmPackage/1?session_token=83af7e620c83a50a18d3eac2f6ed05a3ca0bea62
   Accept: application/octet-stream
   Content-Type: application/json
   range: bytes=0-


The headers **Accept** and **Content-Type** are mandatory.
The header **Range** is optional and allows to download a range of bytes. 

The body of the answer is the file or a part of the file, depending on the usage of the **Range** header.

Using ranges
~~~~~~~~~~~~

In a nutshell:

* bytes=0- means bytes [0; +infinite[ (in other words: the whole file)
* bytes=10-100 means bytes  [10; 100]
* bytes=-100 means bytes [0; 100] It is similar to bytes=0-100

Answered headers
~~~~~~~~~~~~~~~~

* Accept-Ranges: bytes
* Access-Control-Expose-Headers: content-type, content-range, accept-range
* Cache-Control: private, must-revalidate
* Connection: keep-alive
* Content-Length: 504750
* Content-Range: bytes 0-504750/504750
* Content-Transfer-Encoding: binary
* Content-Type: application/octet-stream
* Content-disposition: attachment; filename="b_1_q_0_p_0.jpg"
* Date: Fri, 02 Sep 2016 14:15:23 GMT
* Expires: Mon, 26 Nov 1962 00:00:00 GMT
* Last-Modified: Fri, 02 Sep 2016 13:57:41 +0200
