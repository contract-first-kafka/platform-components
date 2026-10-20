# Registry Designer authoring

The existing Apicurio Registry 3 UI includes Designer when
`spec.app.features.versionMutabilityEnabled: true`. The installed Registry Operator CRD
supports this field. It enables content edits for DRAFT versions only, not finalized
versions. This platform does not enable `resourceDeleteEnabled`.

Open the existing Registry UI (the `spec.ui.ingress.host` on the deployed
`ApicurioRegistry3`), sign in through the existing platform Keycloak, and use Designer
with an authorized developer or administrator account. Create an AsyncAPI draft in a
separate authoring group rather than editing pipeline-owned contracts. Save iterative
changes as drafts. Version compatibility must be checked against the deployed editor;
being able to store an AsyncAPI document does not prove visual-editor support.

Git remains authoritative. Export/download the authored document, review it in the
appropriate API repository, and use the existing validation and publication pipeline.
Do not finalize Designer drafts into the governed release lifecycle or add automatic
publication from drafts. This change adds no publication automation or role changes.

## Removing the standalone installation

The default ApplicationSet no longer includes the standalone designer or its operator.
Because it uses `applicationsSync: create-update` and `preserveResourcesOnDeletion: true`,
existing installations require explicit cleanup after the root reconciles. Verify the
legacy namespace contains only the retired designer before deleting anything. Remove
the two obsolete Argo Applications without cascading deletion; delete the standalone
custom resource while its operator is still running, then remove its Subscription,
installed CSV, and dedicated namespace. Remove its cluster-scoped CRD and any remaining
operator-owned RBAC only after confirming no other instances or installations use them.
Do not change the ApplicationSet deletion policy or remove Apicurio Registry resources.

Reference: [Operator feature configuration](https://www.apicur.io/registry/docs/apicurio-registry/3.1.x/getting-started/assembly-operator-config-reference.html).
