Amazon Web Services
===================

- Introduction
- How to create an account
- Contribute documentation (Francois request)

1. What's Amazon Web Services (AWS)
-----------------------------------

2. Create an account
--------------------

1. Go to `Amazon Web Services <https://aws.amazon.com/>`_.
2. Click on create account, it will redirect you to another page, where if you already have an AWS account can login, otherwise click on create a new account.
3. Fill your information

  3.1 Email, password

    .. image:: images/create-aws-account.png
       :alt: Contact Information

  3.2 Contact information

    .. image:: images/contact-information.png
       :alt: Contact Information

  3.3 Payment information

    .. image:: images/payment-information.png
       :alt: Payment Information


  .. note::
     If you create an account for the first time, you'll have 12 months of Free Tier Access. No 
     charges will be made to your credit or debit cards as long as you don't pass 
     the limits of the `AWS free tier <https://aws.amazon.com/free/>`_.

4. Account verification, you'll receive a phone call asking you to introduce a code through your phone's keypad.
5. Choose a support plan. For more information, see the `Compare AWS Plan Support <https://aws.amazon.com/premiumsupport/compare-plans/>`_.
6. Once your account has been activated you'll receive a confirmation email, after this you'll have full access to the Amazon Web Services.

3. Create a Key pair
--------------------

To access and launch EC2 instances, you'll need a SSH key. Save it in a safe place!

1. Sign in to your AWS account, go to `Amazon Web Services <https://aws.amazon.com/>`_
2. After signing in, you'll be redirected to your `AWS console <https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html>`_.
3. Display all services and select the EC2 service.
4. Under Network & Security, select Key pairs
5. Click on Create Key Pair
6. You'll be prompted for a name
7. A public and private key will be generated, download the private key, remember to save it in a safe place.

4. Deploy GLPI
--------------

1. Sign in to your AWS Marketplace account (the same credentials of your AWS account), go to `AWS Marketplace <https://aws.amazon.com/marketplace/>`_
2. Search GLPI Network, provided by Teclib', there you'll be able to view the product overview, pricing, usage, support and reviews.
3. Click on Continue to Subscribe.

    .. image:: images/GLPI-network-basic.png
       :alt: Continue to subscribe

4. You'll be able to review and accept the Terms and conditions along with the pricing information.
5. After accepting the Terms and subscribing, click on Continue to configuration.
6. \*Here may go the configuration of the instance with the SSH key pair, and the username.
7. After your GLPI instance has been launched on the server, it'll have a public DNS. Retrieve the public DNS.
8. Connect through SSH, for this you'll need the username and the public DNS:

.. code::

   ssh username@ec2-XXX-XX-XXX-X.compute-1.amazonaws.com

Type yes when it ask you to continue connecting.

9. Use the following command to retrieve the login credentials.

    .. image:: images/ssh-login.png
       :alt: SSH login
