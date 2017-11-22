# API models

## User

User accounts in the platform (including administrator ones).

Note that session management is performed by means of a JWT token obtained
through the `api/v1/users/login` endpoint.

User roles are registered in the `user_roles` relationship, and are
checked on certain operations.

Users can follow other users. Followers can see matches in which other users
have participated. This is done through the `followers` relationship.

### Attributes

| Attribute | Type | Description |
| --- | --- | --- | --- |
| **id** | integer | (Primary key) Unique ID of the user |
| username | string | Unique username of the user |
| name | string | (Optional) Real name of the user |
| email | string | Unique email of the user, necessary for any communication (e.g. restoring a password) |
| password | string | Hashed password |
| score | integer | Total score obtained by participating in matches |
| password_reset_token | integer | **Generated on demand**. This token is used to restore the password. It must be **unique and 32 characters long** |
| token_expiration | date | Date(time) in which the `password_reset_token` expires and a new one has to be generated |
| is_deleted | boolean | Flag indicating soft delete |
| delete_date | date | Date in which the user was (soft) deleted |


## Role

Roles of the system.

The roles assigned to each user are checked when performing operations such as
administration tasks.

Note that User-Role relations are stored in the `user_roles` relationship.

### Attributes

| Attribute | Type | Description |
| --- | --- | --- | --- |
| **id** | integer | (Primary key) Unique ID of the role |
| name | string | Unique descriptive name of the role |


## Match

Matches are programming contests created by the administrators of the platform.
Each match has three main periods:

- **Preparation**: from the match is published to the beginning of the match,
users can sign up for matches by participating alone (when possible), creating
a party that other users can join, or joining an existing party
- **Match**: from the beginning to the end of the match, teams can view the
full description of the problem tosolve and submit their solutions. No more
parties can enter the match at this point
- **Evaluation**: after the end of the match, administrators evaluate the
submissions and teams are given a score accordingly

| Attribute | Type | Description |
| --- | --- | --- | --- |
| **id** | integer | (Primary key) Unique ID of the match |
| title | string | Title of the match |
| short_description | string | Short description of the match. Ideally, it should be used to show very little information on the match as to not provide any sort of advantage to the participants until the match begins |
| long_description | string | Long description of the match. As opposed to `short_description`, this one should contain the topic ofthe match and any additional details such as rules (e.g. *use a specific technology or language*). **Should only be shown when the match starts** |
| start_date | date | Date(time) in which the match starts (`long_description` is visible, no more parties can be formed or participate, and submissions are accepted) |
| end_date | date | Date(time) in which the match ends (no more submissions are accepted) |
| min_members | integer | Minimum number of participants in a party |
| max_members | integer | Maximum number of participants in a party |
| slug | string | Unique slug created from the tittle of the match (useful for pretty URLs) |
| is_visible | boolean | Whether the match has been published or not |
| is_deleted | boolean | Flag indicating soft delete |
| delete_date | date | Date in which the match was (soft) deleted |


## MatchParticipant

Match participants and parties are represented with through this model.

Several things to consider:

- The `party_owner_id` indicates the owner/leader of the party. If the value
is the same as `user_id`, then the user is the owner/leader of the party and
may invite others to join them
- In order to invite other users to join a party, the owner must create a token
that can be used to join the party (until a maximum of members is reached).
This token is created on demand when a party is created, and is deleted when
the owner joins a different party
- Users may join the party without knowing the token if the owner marks the
token as public, which in turn shows the party in a *Looking For Group* section

### Attributes

| Attribute | Type | Description |
| --- | --- | --- | --- |
| **user_id** | integer | (Primary key) User ID [Foreign key] |
| **match_id** | integer | (Primary key) Match ID [Foreign key] |
| party_owner_id | integer | ID of the owner of the party the user is currently in [Foreign key] |
| is_participating | boolean | Flag that is set when the match begins indicating whether the user is participating in the match (e.g. the party has the required number of members) |


## PartyToken

Party tokens are used to join a party.

These tokens are created on demand when the user signs up for a match and is
the owner of the party they are currently in. The token is deleted when
the user joins a different party.

### Attributes

| Attribute | Type | Description |
| --- | --- | --- | --- |
| **owner_id** | integer | (Primary key) Owner ID [Foreign key] |
| **match_id** | integer | (Primary key) Match ID [Foreign key] |
| token | string | Unique 32-character random token |
| is_public | boolean | Flag used to specify whether the party can be found through the *Looking For Group* section. If this value is False (default), the owner
must provide the link to join the party manually |


## Submission

Submissions are the resulting projects created by the participants at the end of
a match.

Each party can have one submission per match, and this submission must be
created by the party leader.

Submissions can be edited until the match is closed.

### Attributes

| Attribute | Type | Description |
| --- | --- | --- | --- |
| **id** | integer | (Primary key) Unique ID of the submission |
| title | string | Short title of the project |
| description | string | Long description of the project |
| url | string | URL from which the project can be downloaded |
| match_id | integer | ID of the match [Foreign key] |
| party_owner_id | integer | ID of the leader/owner of the party (used to identify the party) [Foreign key] |
