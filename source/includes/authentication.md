# Authentication

In order to use our API endpoints, you will need to have an API token created and
for this token to be granted the relevant permissions. To authenticate requests we
require you to pass us this token in the form of an HTTP header called
`Authorization` with the value of `Token token=YOURAPITOKENHERE`.

<aside class="warning">You must set the <strong>Authorization</strong> as detailed above</aside>

Failure to supply a recognised key in the correct format will result in a 403 HTTP response code and an error message:
HTTP Token: Access denied.

Your API token should have the following format consisting of an API key followed by a API secret, separated with a colon like so:
abcdefghijklmnopqrst:uvwxyz1234567890abcdefghijklmnop


