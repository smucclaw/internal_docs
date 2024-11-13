# DevOps

When we deliver a use case, the runtimes need to be exposed for third-party use, and maintained against bitrot.

We use AWS for this. We have the insurance LE backend server running in a production instance somewhere.

The PDPA vue server also runs in a production instance.

[https://l4-documentation.readthedocs.io/en/23.06.04/docs/links-backend.html](https://l4-documentation.readthedocs.io/en/23.06.04/docs/links-backend.html)

[Docs for how to set up some of the Natural L4 server-side stuff](https://github.com/smucclaw/gsheet/tree/main/natural4-server)

We separate dev from production servers. There is a dev server that is
available for people to work on, if their laptops aren't sufficient.
Access to this server is controlled by IP for reasons best explained
by the university IT department, so every time you want to log in to
it, you have to ask somebody who has AWS console credentials to go
update the security group ACL.
