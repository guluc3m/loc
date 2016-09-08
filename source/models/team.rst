Team
====

Team participating in a match. The process of creation of a team is as follows:

1. An user creates a team for a specific match.
2. The owner is given an :py:attr:`invite_token` that they can give other users
   to join the team
3. Each time an user joins the team, a record is created in the `team_members`
   database relationship (including the owner), when the number of members is
   greater than :py:attr:`min_team` in the specific `Match`, the team can
   participate
4. Users will not be able to join the team once the :py:attr:`max_team` set
   in `Match` has been reached

Teams are coupled to a single `Match`.

.. py:class:: Team

   .. py:attribute:: id

      *int* (primary key)

      Unique ID of the Team

   .. py:attribute:: owner_id

      *int* (foreign key to `User`)

      Owner of the team, used mainly for communication purposes

   .. py:attribute:: match_id

      *int* (foreign key to `Match`)

      Match in which the team is participating

   .. py:attribute:: invite_token

      *string*

      Unique token that can be used to invite other users to the team

   .. py:attribute:: position

      *int*

      Position given to the team after the match is over

   .. py:attribute:: is_participating

      *boolean*

      Flag indicating if the team is participating. This is automatically set to
      `True` when the team reaches the minimum number of participants

   .. py:attribute:: is_disqualified

      *boolean*

      Flag indicating if the team has been disqualified. Should only be used in
      special cases

