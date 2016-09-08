Submission
==========

Contains the basic information of a project submitted by one team.


.. py:class:: Team

   .. py:attribute:: id

      *int* (primary key)

      Unique ID of the Submission

   .. py:attribute:: title

      *string*

      Title of the project. Try to be creative!

   .. py:attribute:: description

      *string*

      Description of the project. May be used to show the basic idea behind the
      project

   .. py:attribute:: url

      *string*

      Url to the project source code. This may be a repository or a meta site
      that includes a presentation of the project, or instructions, in
      addition to the source code

   .. py:attribute:: team_id

      *int* (foreign key to `Team`)

      ID of the team that owns this submission
