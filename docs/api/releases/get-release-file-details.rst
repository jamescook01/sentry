.. this file is auto generated. do not edit

Retrieve a File
===============

.. sentry:api-endpoint:: get-release-file-details

    Return details on an individual file within a release.  This does
    not actually return the contents of the file, just the associated
    metadata.

    :pparam string organization_slug: the slug of the organization the
                                      release belongs to.
    :pparam string project_slug: the slug of the project to retrieve the
                                 file of.
    :pparam string version: the version identifier of the release.
    :pparam string file_id: the ID of the file to retrieve.
    :auth: required

    :http-method: GET
    :http-path: /api/0/projects/{organization_slug}/{project_slug}/releases/{version}/files/{file_id}/

Example
-------


.. sentry:api-scenario:: RetrieveReleaseFile
