# Authentification

For the moment, our REST API use Apache to provide secure authentification.

You need to add your restrictions in the `<Location /ocsapi></Location>`. You can restrict access by IP or by user / pass authentification. Please, Refer to apache documentation for this part.

In the future we plan to use a restful process and stop rely on apache configuration.

By default, the API can be access trough : 
`http://myocsserver/ocsapi/v1/my/routes`