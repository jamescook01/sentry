.. this file is auto generated. do not edit

Bulk Mutate a List of Aggregates
================================

.. sentry:api-endpoint:: put-project-group-index

    Bulk mutate various attributes on aggregates.  The list of groups
    to modify is given through the `id` query parameter.  It is repeated
    for each group that should be modified.
    
    - For non-status updates, the `id` query parameter is required.
    - For status updates, the `id` query parameter may be omitted
      for a batch "update all" query.
    - An optional `status` query parameter may be used to restrict
      mutations to only events with the given status.
    
    The following attributes can be modified and are supplied as
    JSON object in the body:
    
    If any ids are out of scope this operation will succeed without
    any data mutation.
    
    :qparam int id: a list of IDs of the groups to be mutated.  This
                    parameter shall be repeated for each group.  It
                    is optional only if a status is mutated in which
                    case an implicit `update all` is assumed.
    :qparam string status: optionally limits the query to groups of the
                           specified status.  Valid values are
                           ``"resolved"``, ``"unresolved"`` and
                           ``"muted"``.
    :pparam string organization_slug: the slug of the organization the
                                      groups belong to.
    :pparam string project_slug: the slug of the project the groups
                                 belong to.
    :param string status: the new status for the groups.  Valid values
                          are ``"resolved"``, ``"unresolved"`` and
                          ``"muted"``.
    :param boolean isPublic: sets the group to public or private.
    :param boolean merge: allows to merge or unmerge different groups.
    :param boolean hasSeen: in case this API call is invoked with a user
                            context this allows changing of the flag
                            that indicates if the user has seen the
                            event.
    :param boolean isBookmarked: in case this API call is invoked with a
                                 user context this allows changing of
                                 the bookmark flag.
    :auth: required

    :http-method: PUT
    :http-path: /api/0/projects/{organization_slug}/{project_slug}/groups/

Example
-------


.. sentry:api-scenario:: BulkUpdateAggregates
