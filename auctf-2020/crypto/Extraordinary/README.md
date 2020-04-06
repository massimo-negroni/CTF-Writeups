Extraordinary
=============
658 points
----------

this chall from an encrypted text:
```
b'6 \ x1d \ x0cT * \ x12 \ x18V \ x05 \ x13c1R \ x07u # \ x021Jq \ x05 \ x02n \ x03t% 1 \\ \\ x04 @ V7P x17aN '
```
and a Netcat connection.

If we connect we simply find a machine which, given an input string, returns an output string.
We certainly know that the first part of the flag is 'auctf {' because it is the flag format, 
and by inserting some input we realize that the algorithm always returns an output message except for the correct flag.
we know that the flag contains only printable characters so,
just try the various characters one by one in the oracle until the flag is completed.
This chall was very quick to complete, I didn't feel the need to write a script.
