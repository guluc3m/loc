Match
=====

Matches are programming contests created by the administrators of the platform.
Each match has three main periods:

- **Preparation**: from the moment the match is published to the beginning of
  the match (see :py:attr:`start_date`), users can register in matches as
  part of a team in order to participate
- **Match**: from the beginning to the end of the match (see
  :py:attr:`start_date` and :py:attr:`end_date`), teams can view the full
  description of the problem and submit their solutions. No more teams
  can enter the match at this point
- **Evaluation**: after the end of the match (see :py:attr:`end_date`),
  administrators evaluate the submissions and teams are given a score
  accordingly


.. py:class:: Match

   .. py:attribute:: id

      *int* (primary key)

      Unique ID for the match

   .. py:attribute:: title

      *string*

      Title of the match

   .. py:attribute:: short_description

      *string*

      Short description of the match. Ideally, it should be used to show very
      little information on the match as to not provide any advantage to the
      participants until the actual match begins

   .. py:attribute:: long_description

      *string*

      Long description of the match. As opposed to :py:attr:`short_description`,
      this one should contain the topic of the match and any additional details
      such as rules (e.g. *use a specific technology or language*).

      **Should only be shown when the match starts**

   .. py:attribute:: start_date

      *date*

      Date(time) when the match starts (:py:attr:`long_description` is visible
      and submissions are accepted)

   .. py:attribute:: end_date

      *date*

      Date(time) when the match ends (no more submissions are accepted)

   .. py:attribute:: min_team

      *int*

      Minimum number of participants in a team

   .. py:attribute:: max_team

      *max*

      Maximum number of participants in a team

   .. py:attribute:: slug

      *string*

      Unique slug created from the title of the match (useful for pretty URLs)

   .. py:attribute:: is_visible

      *boolean*

      Whether the match has been published or not

   .. py:attribute:: is_deleted

      *boolean*

      Whether the user record has been deleted or not (soft delete)

   .. py:attribute:: delete_date

      *date*

      When the user record was *deleted* (soft delete)
