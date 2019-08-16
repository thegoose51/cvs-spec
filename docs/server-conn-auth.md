# How to Connect to and Authenticate Oneself to the CVS Server

Connection and authentication occurs before the CVS protocol itself is started. There are several ways to connect.

## server

If the client has a way to execute commands on the server, and provide input to the commands and output from them, then it can connect that way. This could be the usual rsh (port 514) protocol, Kerberos rsh, SSH, or any similar mechanism. The client may allow the user to specify the name of the server program; the default is **`cvs`**. It is invoked with one argument, `server`. Once it invokes the server, the client proceeds to start the **cvs** protocol.

## kserver

The kerberized server listens on a port (in the current implementation, by having inetd call `cvs kserver`) which defaults to 1999. The client connects, sends the usual kerberos authentication information, and then starts the **cvs** protocol.

!!! Note
    Port 1999 is officially registered for another use, and in any event one cannot register more than one port for CVS, so GSS-API (see below) is recommended instead of kserver as a way to support kerberos.

## pserver

The name `pserver` is somewhat confusing. It refers to both a generic framework which allows the CVS protocol to support several authentication mechanisms, and a name for a specific mechanism which transfers a username and a cleartext password. Servers need not support all mechanisms, and in fact servers will typically want to support only those mechanisms which meet the relevant security needs.

The pserver server listens on a port (in the current implementation, by having inetd call `cvs pserver`) which defaults to 2401 (this port is officially registered). The client connects, and sends the following:

- the string `BEGIN AUTH REQUEST`, a linefeed,
- the cvs root, a linefeed,
- the username, a linefeed,
- the password trivially encoded (see Password scrambling), a linefeed,
- the string `END AUTH REQUEST`, and a linefeed.

The client must send the identical string for cvs root both here and later in the Root request of the cvs protocol itself. Servers are encouraged to enforce this restriction. The possible server responses (each of which is followed by a linefeed) are the following. Note that although there is a small similarity between this authentication protocol and the **cvs** protocol, they are separate.

`I LOVE YOU`

The authentication is successful. The client proceeds with the cvs protocol itself.

`I HATE YOU`

The authentication fails. After sending this response, the server may close the connection. It is up to the server to decide whether to give this response, which is generic, or a more specific response using `E` and/or `error`.

`E` _`text`_

Provide a message for the user. After this reponse, the authentication protocol continues with another response. Typically the server will provide a series of `E` responses followed by `error`.

!!! warning "Compatibility Note"
    **`cvs`** 1.9.10 and older clients will print `unrecognized auth response` and _`text`_, and then exit, upon receiving this response.

`error` **_`code`_** _`text`_

The authentication fails. After sending this response, the server may close the connection. The **_`code`_** is a code describing why it failed, intended for computer consumption. The only code currently defined is `0` which is nonspecific, but clients must silently treat any unrecognized codes as nonspecific. The text should be supplied to the user.

!!! warning "Compatibility Note"
    **`cvs`** 1.9.10 and older clients will print `unrecognized auth response` and _`text`_, and then exit, upon receiving this response.

!!! note
    _`text`_ for this response, or the _`text`_ in an `E` response, is not designed for machine parsing. More vigorous use of code, or future extensions, will be needed to prove a cleaner machine-parseable indication of what the error was.

If the client wishes to merely authenticate without starting the **cvs** protocol, the procedure is the same, except:

- `BEGIN AUTH REQUEST` is replaced with `BEGIN VERIFICATION REQUEST`,
- `END AUTH REQUEST` is replaced with `END VERIFICATION REQUEST`,
- Upon receipt of `I LOVE YOU` the connection is closed instead of continuing.

Another mechanism is GSSAPI authentication. GSSAPI is a generic interface to security services such as kerberos. GSSAPI is specified in RFC2078 (GSSAPI version 2) and RFC1508 (GSSAPI version 1); we are not aware of differences between the two which affect the protocol in incompatible ways, so we make no attempt to specify one version or the other. The procedure here is to start with `BEGIN GSSAPI REQUEST`. GSSAPI authentication information is then exchanged between the client and the server. Each packet of information consists of a two byte big endian length, followed by that many bytes of data. After the GSSAPI authentication is complete, the server continues with the responses described above (`I LOVE YOU`, etc.).

## Future Possibilities

There are nearly an unlimited number of ways to connect and authenticate. One might want to allow access based on IP address (similar to the usual rsh protocol but with different/no restrictions on ports < 1024), to adopt mechanisms such as Pluggable Authentication Modules (PAM), to allow users to run their own servers under their own usernames without root access, or any number of other possibilities. The way to add future mechanisms, for the most part, should be to continue to use port 2401, but to use different strings in place of `BEGIN AUTH REQUEST`.
