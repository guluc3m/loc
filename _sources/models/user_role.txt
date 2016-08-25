UserRole
========

This model represents the association between an user and the roles they have
been assigned.

.. py:class:: UserRole

   .. py:attribute:: id

      *int* (primary key)

      Unique ID for the association

   .. py:attribute:: user_id

      *int* (Foreign key to :doc:`/models/user`)

      ID of the user for which the association is defined

   .. py:attribute:: role_id

      *int* (foreign key to :doc:`/models/role`)

      ID of the role for which the association is defined
