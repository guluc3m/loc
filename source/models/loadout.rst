Loadout
=======

`Team` *templates*. Users may create loadouts with users they enjoy working with
in order to send them a direct invitation when creating a team.

The process is as follows:

1. User creates a loadout and adds other users to it (records in the
   `loadout_members` database relationship)
2. When creating a team, choose a loadout and send invitation links to each of
   the users in the loadout
3. The rest of the process is the same as when creating a `Team`

.. py:class:: Loadout

   .. py:attribute:: id

      *int* (primary key)

      Unique ID of the Loadout

   .. py:attribute:: name

      *string*

      Name given to the loadout

   .. py:attribute:: owner_id

      *int* (foreign key to `User`)

      Owner of the loadout
