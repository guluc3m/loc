User
====

User accounts in the platform (including administrator ones). Note that session
management is performed by means of a JWT token obtained through the
:doc:`/api/users/login` endpoint.

User roles are registered in the `user_roles` database relationship, and are
checked on certain operations.

Users can follow other users. Followers can see matches in which other users
have participated. This is is done through the `followers` database
relationship.

.. py:class:: User

   .. py:attribute:: id

      *int* (primary key)

      Unique ID of the User

   .. py:attribute:: username

      *string*

      Unique username of the User

   .. py:attribute:: email

      *string*

      Unique email of the User. Communication with the user will be performed
      through this address (e.g. restoring a password)

   .. py:attribute:: password

      *string*

      Stored password of the User (hashed)

   .. py:attribute:: score

      *int*

      Total score obtained by participating in matches

   .. py:attribute:: password_reset_token

      *int*

      **Generated on demand**. This token is used to restore the password.
      It must be **unique and 32 characters long**

   .. py:attribute:: token_expiration

      *date*

      Date(time) in which the :py:attr:`password_reset_token` expires and a
      new one has to be generated

   .. py:attribute:: is_deleted

      *boolean*

      Whether the user record has been deleted or not (soft delete)

   .. py:attribute:: delete_date

      *date*

      When the user record was *deleted* (soft delete)
