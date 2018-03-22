Update user
===========

This works only when a user has UPDATE right on users. a guest profile cannot update himself from the API currently, see `GLPI issue #2568 <https://github.com/glpi-project/glpi/issues/2568>`_.

The agent may update user information like phone number, emails, name.

This can be done with the endpoint ``User``. Only fields requiring an update should be specified. 

PUT apirest.php/User/:id

.. code:: json

   {
       "input": {
           "firstname": "new firstname",
           "realname": "new realname",
           "phone": "0123456789",
           "phone2": "0987654321",
           "_useremails": { 
               "0": "email1@example.com",
               "1": "email2@example.com"
           },
           "_default_email": "1",
       }
   }
