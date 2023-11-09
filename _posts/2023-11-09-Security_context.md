---
category: K8s
tags: [CKA]
usemath: [latex, ascii] 
---

# Configure a Security Context for a Pod or Container

A security context defines privilege and access control settings for a Pod or Container. Security context settings include, but are not limited to

- Discretionary Access Control: Permission to access an object, like a file, is based on user ID (UID) and group ID (GID).
- Running as privileged or unprivileged.
- <code>allowPrivilegeEscalation</code>: Controls whether a process can gain more privileges than its parent process. This bool directly controls whether the <code>no_new_privs</code> flag gets set on the container process. <code>allowPrivilegeEscalation</code> is always true when the container:
  - is run as privileged, or
  - has <code>CAP_SYS_ADMIN</code>
- <code>readOnlyRootFilesystem</code>: Mounts the container's root filesystem as read-only.



> 



