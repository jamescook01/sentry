.. this file is auto generated. do not edit

Create a New Release
====================

.. sentry:api-endpoint:: post-project-releases

    Create a new release for the given project.  Releases are used by
    Sentry to improve it's error reporting abilities by correlating
    first seen events with the release that might have introduced the
    problem.

    Releases are also necessary for sourcemaps and other debug features
    that require manual upload for functioning well.

    :pparam string organization_slug: the slug of the organization the
                                      release belongs to.
    :pparam string project_slug: the slug of the project to create a
                                 release for.
    :param string version: a version identifier for this release.  Can
                           be a version number, a commit hash etc.
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

    :http-method: POST
    :http-path: /api/0/projects/{organization_slug}/{project_slug}/releases/

Example
-------


.. sentry:api-scenario:: CreateNewRelease
