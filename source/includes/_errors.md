# Errors

The API uses the following error codes:


Error&nbsp;Code | Meaning
---------- | -------
401 | Unauthorized -- Your API key is wrong or missing.
404 | Not Found -- The requested object could not be found.
422 | Unprocessable Entity -- Your request is invalid in some way, see the <code>message</code> attribute for a clue.
500 | Internal Server Error -- We have a problem with our server. Try again later and if the problem persists, contact us.
