.. this file is auto generated. do not edit

Update a Release
================

.. sentry:api-endpoint:: put-release-details

    Update a release.  This can change some metadata associated with
    the release (the ref, url, and dates).

    :pparam string organization_slug: the slug of the organization the
                                      release belongs to.
    :pparam string project_slug: the slug of the project to change the
                                 release of.
    :pparam string version: the version identifier of the release.
    :param string ref: an optional commit reference.  This is useful if
                       a tagged version has been provided.
    :param url url: a URL that points to the release.  This can be the
                    path to an online interface to the sourcecode
                    for instance.
    :param datetime dateStarted: an optional date that indicates when the
                                 release process started.
    :param datetime dateReleased: an optional date that indicates when
                                  the release went live.  If not provided
                                  the current time is assumed.
    :auth: required

    :http-method: PUT
    :http-path: /api/0/projects/{organization_slug}/{project_slug}/releases/{version}/

Example
-------


.. sentry:api-scenario:: UpdateRelease
