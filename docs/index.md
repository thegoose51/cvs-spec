# CVS Version Control Specification

## Manual

### How to Use and Administer CVS Version 1.12.13

- [Overview](man-overview.md): An introduction to CVS
- [Repository](man-repository.md): Where all your sources are stored
- [Starting a new project](man-new-project.md): Starting a project with CVS
- [Revisions](man-revisions.md): Numeric and symbolic names for revisions
- [Branching and merging](man-branching-merging.md): Diverging/rejoining branches of development
- [Recursive behavior](man-recursive.md): CVS descends directories
- [Adding and removing](man-adding-removing.md): Adding/removing/renaming files/directories
- [History browsing](man-history.md): Viewing the history of files in various ways

### CVS and the Real World

- Binary files: CVS can handle binary files
- Multiple developers: How CVS helps a group of developers
- Revision management: Policy questions for revision management
- Keyword substitution: CVS can include the revision inside the file
- Tracking sources: Tracking third-party sources
- Builds: Issues related to CVS and builds
- Special Files: Devices, links and other non-regular files

### References

- CVS commands: CVS commands share some things
- Invoking CVS: Quick reference to CVS commands
- Administrative files: Reference manual for the Administrative files
- Environment variables: All environment variables which affect CVS
- Compatibility: Upgrading CVS versions
- Troubleshooting: Some tips when nothing works
- Credits: Some of the contributors to this manual
- BUGS: Dealing with bugs in CVS or this manual
- CVS command list: Alphabetical list of all CVS commands
- Index: Index

## CVS Client/Server

These documents describe the [client/server](server-introduction.md) protocol used by CVS. It does not describe how to use or administer client/server CVS; for that, see the regular CVS [manual](#manual). This is version 1.12.13 of the protocol specification—See [Introduction](server-introduction.md), for more on what this version number means.

- [Introduction](server-introduction.md): What is CVS and what is the client/server protocol for?
- [Goals](server-goals.md): Basic design decisions, requirements, scope, etc.
- [Connection and Authentication](server-conn-auth.md): Various ways to connect to the server
- [Password scrambling](server-password.md): Scrambling used by pserver
- [Protocol](server-protocol.md): Complete description of the protocol
- [Protocol Notes](server-protocol-notes.md): Possible enhancements, limitations, etc. of the protocol
