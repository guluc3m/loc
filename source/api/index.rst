LoC API
=======

The API follows the JSend specification found at
https://labs.omniti.com/labs/jsend . Apart from the HTTP status code, each
endpoint will return a *success*, *fail* or *error* response according to the
specification.

If a message needs to be returned, the string will be localized according to
the HTTP locale header when possible. Otherwise, the message will be displayed
in **English** by default.


.. toctree::
   :maxdepth: 2

   matches/index
   teams/index
   users/index
