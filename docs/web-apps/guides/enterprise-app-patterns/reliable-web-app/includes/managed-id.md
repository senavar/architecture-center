---
author: ssumner
ms.author: ssumner
ms.date: 10/15/2024
ms.topic: include
ms.service: azure-architecture-center
---
Use [Managed Identities](/entra/identity/managed-identities-azure-resources/overview-for-developers) for all Azure services that support managed identities. A managed identity allows Azure resources ([workload identities](/entra/workload-id/workload-identities-overview)) to authenticate to and interact with other Azure services without managing credentials. Hybrid and legacy systems can keep on-premises authentication solutions to simplify the migration but should transition to managed identities as soon as possible. To implement managed identities, follow these recommendations:

- *Pick the right type of managed identity*. Prefer user-assigned managed identities when you have two or more Azure resources that need the same set of permissions. This setup is more efficient than creating system-assigned managed identities for each of those resources and assigning the same permissions to all of them. Otherwise, use system-assigned managed identities.

- *Configure least privileges.* Use [Azure RBAC](/azure/role-based-access-control/best-practices) to grant only the permissions that are critical for the operations, such as CRUD actions in databases or accessing secrets. Workload identity permissions are persistent, so you can't provide just-in-time or short-term permissions to workload identities. If Azure RBAC doesn't cover a specific scenario, supplement Azure RBAC with Azure-service level access policies.

- *Secure remaining secrets.* Store any remaining secrets in [Azure Key Vault](/azure/key-vault/secrets/about-secrets). Load secrets from Key Vault at application startup instead of during each HTTP request. High-frequency access within HTTP requests can exceed [Key Vault transaction limits](/azure/key-vault/general/service-limits#secrets-managed-storage-account-keys-and-vault-transactions). Store application configurations in [Azure App Configuration](/azure/azure-app-configuration/overview).
