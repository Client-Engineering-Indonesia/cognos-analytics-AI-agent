# Cognos Analytics REST API Reference

> **Base URL:** `http://<host>:9300/api/v1`  
> **Spec URL:** `http://ibmdemos:9300/api/api-docs/swagger.json`  
> **Official Docs:** https://www.ibm.com/docs/en/cognos-analytics/12.1.0?topic=apis-rest-api  
> **OpenAPI version:** 3.0.1

---

## Table of Contents

1. [Authentication](#1-authentication)
2. [Session](#2-session)
3. [Content Store](#3-content-store)
4. [Data Sources & Connections](#4-data-sources--connections)
5. [Metadata (Schemas & Tables)](#5-metadata-schemas--tables)
6. [Modules (Data Modules)](#6-modules-data-modules)
7. [Files (Uploaded Data)](#7-files-uploaded-data)
8. [Accounts (Users, Groups, Roles, Capabilities)](#8-accounts-users-groups-roles--capabilities)
9. [Security (API Keys)](#9-security-api-keys)
10. [Configuration (Namespaces & Keys)](#10-configuration-namespaces--keys)
11. [Customizations, Themes & Extensions](#11-customizations-themes--extensions)
12. [Storage (Cloud Storage)](#12-storage-cloud-storage)
13. [Tenants](#13-tenants)
14. [Common Patterns & Tips](#14-common-patterns--tips)

---

## 1. Authentication

Cognos uses **session-cookie-based** auth. All requests require an active session unless you use an API key.

### Security schemes

| Scheme | How it works |
|--------|-------------|
| `CAM` (cookie) | Establish a session via `PUT /session`, then every subsequent request carries the `ibmCognosAuthn` session cookie automatically. |
| API Key | Create a key via `POST /security/login_api_keys` and pass it in the `Authorization` header as `Bearer <key>`. |

### Log in (establish a session)

```
PUT /api/v1/session
Content-Type: application/json
```

**Request body** (`CreateSession`):
```json
{
  "parameters": [
    { "name": "CAMNamespace",  "value": "CognosEx" },
    { "name": "CAMUsername",   "value": "admin" },
    { "name": "CAMPassword",   "value": "password" }
  ]
}
```

**Responses:**
| Code | Meaning |
|------|---------|
| `201` | Session created — returns `Session` object |
| `200` | Session updated |
| `401` | Auth required / bad credentials |
| `403` | Invalid namespace |

The response sets an `ibmCognosAuthn` cookie. Store and replay it for all subsequent calls.

**Session object fields:**

| Field | Type | Description |
|-------|------|-------------|
| `session_key` | string | CAM session key |
| `cafContextId` | string | CAF context ID |
| `anonymous` | boolean | `true` if guest |
| `generation` | integer | Passport generation |
| `url` | string | Logon URL |

---

## 2. Session

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/session` | Get current session info |
| `PUT` | `/session` | Log in / update session |
| `DELETE` | `/session` | Log out |

### Check who is logged in

```
GET /api/v1/session
```

Returns the `Session` object (see above).

### Log out

```
DELETE /api/v1/session
```

Returns `204 No Content`.

---

## 3. Content Store

The content store holds all Cognos objects: folders, reports, dashboards, data modules, etc.

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/content` | Get root objects (namespaces) |
| `GET` | `/content/{id}` | Get object by ID |
| `PUT` | `/content/{id}` | Update (rename/re-describe) an object |
| `DELETE` | `/content/{id}` | Delete an object |
| `GET` | `/content/{id}/items` | List children of a container |
| `POST` | `/content/{id}/items` | Create a child object |
| `POST` | `/content/copy` | Copy an object (and descendants) |
| `PUT` | `/content/move` | Move an object (and descendants) |

### Get root objects

```
GET /api/v1/content
```

Returns the top-level namespace list (e.g. the `Team content` / `My content` roots).

### Get a specific object

```
GET /api/v1/content/{id}?fields=defaultName,type,ancestors
```

**Query parameters:**

| Param | Description |
|-------|-------------|
| `fields` | Comma-separated extra fields (e.g. `defaultName,type,ancestors,permissions`) |

### List children of a folder

```
GET /api/v1/content/{id}/items?nav_filter=true
```

| Param | Description |
|-------|-------------|
| `fields` | Extra fields to return |
| `nav_filter` | `true` to hide system/hidden objects |

### Create an object inside a folder

```
POST /api/v1/content/{parentId}/items
Content-Type: application/json
```

**Request body** (`NewContent`):
```json
{
  "type": "folder",
  "defaultName": "My New Folder"
}
```

The `type` field is **required**. Common types: `folder`, `report`, `dashboard`, `exploration`.

### Update (rename) an object

```
PUT /api/v1/content/{id}
Content-Type: application/json
```

**Request body** (`UpdateContent`):
```json
{
  "type": "folder",
  "defaultName": "Renamed Folder"
}
```

### Delete an object

```
DELETE /api/v1/content/{id}?recursive=true&force=false
```

| Param | Description |
|-------|-------------|
| `recursive` | Delete all descendants too |
| `force` | Force-delete even if dependents exist |

Returns `204 No Content`.

### Copy an object

```
POST /api/v1/content/copy
Content-Type: application/json
```

**Request body** (`CopyObject`):
```json
{
  "id": "<source-id>",
  "targetId": "<destination-folder-id>"
}
```

### Move an object

```
PUT /api/v1/content/move
Content-Type: application/json
```

**Request body** (`MoveObject`):
```json
{
  "id": "<source-id>",
  "targetId": "<destination-folder-id>"
}
```

---

## 4. Data Sources & Connections

A **data source** holds one or more **connections**, each of which has one or more **signons** (credentials).

### Data Sources

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/datasources` | List all data sources |
| `POST` | `/datasources` | Create a new data source |
| `GET` | `/datasources/{id}` | Get a data source |
| `PUT` | `/datasources/{id}` | Update a data source |
| `DELETE` | `/datasources/{id}` | Delete a data source |
| `POST` | `/datasources/test` | Test a connection with credentials |

### Create a data source (with connection & signon in one call)

```
POST /api/v1/datasources
Content-Type: application/json
```

**Request body** (`NewDataSource`):
```json
{
  "defaultName": "My DB",
  "connections": [
    {
      "defaultName": "primary",
      "type": "JDBC",
      "jdbcUrl": "jdbc:sqlserver://myserver:1433;databaseName=mydb",
      "signons": [
        {
          "credentials": {
            "username": "sa",
            "password": "secret"
          }
        }
      ]
    }
  ]
}
```

### Test a connection

```
POST /api/v1/datasources/test
Content-Type: application/json
```

Pass a `TestConnection` body with `datasourceId`, `connectionId`, `username`, and `password`.

---

### Connections

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/datasources/{dsId}/connections` | List connections |
| `POST` | `/datasources/{dsId}/connections` | Add a connection |
| `GET` | `/datasources/{dsId}/connections/{connId}` | Get a connection |
| `PUT` | `/datasources/{dsId}/connections/{connId}` | Update a connection |
| `DELETE` | `/datasources/{dsId}/connections/{connId}` | Delete a connection |

---

### Signons (credentials)

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/datasources/{dsId}/connections/{connId}/signons` | List signons |
| `POST` | `/datasources/{dsId}/connections/{connId}/signons` | Add a signon |
| `GET` | `/datasources/{dsId}/connections/{connId}/signons/{signonId}` | Get a signon |
| `PUT` | `/datasources/{dsId}/connections/{connId}/signons/{signonId}` | Update a signon |
| `DELETE` | `/datasources/{dsId}/connections/{connId}/signons/{signonId}` | Delete a signon |

---

## 5. Metadata (Schemas & Tables)

After creating a data source, use these endpoints to inspect the available schemas and tables.

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/datasources/{dsId}/connections/{connId}/signons/{signonId}/schemas` | List available schemas |
| `POST` | `/datasources/{dsId}/connections/{connId}/signons/{signonId}/schemas` | Create a schema object (for metadata import) |
| `GET` | `/datasources/{dsId}/connections/{connId}/signons/{signonId}/schemas/{schemaId}` | Get schema spec |
| `PUT` | `/datasources/{dsId}/connections/{connId}/signons/{signonId}/schemas/{schemaId}` | Update schema import definition |
| `DELETE` | `/datasources/{dsId}/connections/{connId}/signons/{signonId}/schemas/{schemaId}` | Delete schema and clear loaded metadata |
| `GET` | `/datasources/{dsId}/connections/{connId}/signons/{signonId}/schemas/tables?schemaid={id}` | List tables in a schema |

### Metadata Import (async)

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/datasources/import_metadata` | Start a metadata import job |
| `GET` | `/datasources/import/tasks/{taskid}` | Poll import task status |
| `DELETE` | `/datasources/import/tasks/{taskid}` | Cancel an import task |

**Workflow:**
1. Call `POST /datasources/import_metadata` → get back a `taskid`.
2. Poll `GET /datasources/import/tasks/{taskid}` until `status` is `complete` or `failed`.
3. The `X-CA-Affinity` header returned by step 1 must be forwarded on step 2 & 3.

---

## 6. Modules (Data Modules)

Data modules are the semantic layer in Cognos — they sit on top of data sources and expose query subjects and items to reports and dashboards.

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/modules` | List all modules |
| `POST` | `/modules?location={folderId}` | Create a module |
| `GET` | `/modules/{moduleId}` | Get the full module definition |
| `PUT` | `/modules/{moduleId}` | Replace a module definition (full PUT) |
| `DELETE` | `/modules/{moduleId}` | Delete a module |
| `GET` | `/modules/{moduleId}/metadata` | Get resolved metadata (tables, columns, relationships) |

### Module object fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Content store ID |
| `name` | string | Internal name |
| `label` | string | Display label |
| `lastModified` | string | ISO timestamp |
| `defaultDescription` | string | Optional description |

### Minimal module definition (for creation)

```json
{
  "version": 3,
  "container": "dataSets",
  "useSpec": [
    {
      "identifier": "myDS",
      "type": "dataSource",
      "storeID": "<datasource-content-store-id>"
    }
  ],
  "querySubject": []
}
```

### Get resolved metadata

```
GET /api/v1/modules/{moduleId}/metadata
```

Returns fully-expanded view: tables (query subjects), columns (query items), data source bindings, and relationships. Use this to understand what is available for reporting.

---

## 7. Files (Uploaded Data)

Upload CSV / Excel files directly into Cognos as data sets.

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/files` | List imported files |
| `POST` | `/files/import` | Initiate a new file import |
| `PUT` | `/files/import/{id}` | Update an existing file import |
| `PUT` | `/files/import/segment/{data_collector_id}` | Upload a file segment (chunked upload) |
| `GET` | `/files/import/tasks` | List import tasks |
| `GET` | `/files/import/tasks/{taskid}` | Get a specific import task |
| `DELETE` | `/files/import/tasks/{taskid}` | Cancel an import task |
| `DELETE` | `/files/{id}` | Delete an imported file |
| `GET` | `/files/assets` | Get uploaded file details (optionally filter by `owner_id`) |

### File upload query parameters

| Param | Where | Description |
|-------|-------|-------------|
| `append` | query | `true` to append rows to existing file |
| `filename` | query | Target filename |
| `index` | query | Segment index (0-based, for chunked upload) |
| `X-CA-Affinity` | header | Affinity token from initiate response; required for segment uploads and task polling |

---

## 8. Accounts (Users, Groups, Roles & Capabilities)

### Users

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/users?identifier={uid}` | Search users by identifier |
| `POST` | `/users?namespace={ns}` | Add a user to a namespace |
| `GET` | `/users/{id}` | Get a user |
| `PUT` | `/users/{id}` | Update a user |
| `DELETE` | `/users/{id}` | Delete a user |
| `POST` | `/users/{id}/copy_profile` | Copy one user's profile to others |
| `POST` | `/users/import` | Bulk import users from CSV |

### Groups

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/groups?parent_id={id}&page={n}` | List groups |
| `GET` | `/groups/{id}` | Get a group |
| `POST` | `/groups/{id}` | Create a child group |
| `PUT` | `/groups/{id}` | Update a group |
| `DELETE` | `/groups/{id}` | Delete a group |
| `GET` | `/groups/{id}/members` | List group members |
| `POST` | `/groups/{id}/members` | Add a member |
| `DELETE` | `/groups/{id}/members/{type}/{member_id}` | Remove a member |
| `POST` | `/groups/import` | Bulk import groups from CSV |

### Roles

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/roles?parent_id={id}` | List roles |
| `GET` | `/roles/{id}` | Get a role |
| `POST` | `/roles/{id}` | Create a child role |
| `PUT` | `/roles/{id}` | Update a role |
| `DELETE` | `/roles/{id}` | Delete a role |
| `GET` | `/roles/{id}/members` | List role members |
| `POST` | `/roles/{id}/members` | Add a member |
| `POST` | `/roles/{id}/members/users?identifier={uid}` | Add a user by identifier |
| `DELETE` | `/roles/{id}/members/users?identifier={uid}` | Remove a user by identifier |
| `DELETE` | `/roles/{id}/members/{type}/{member_id}` | Remove a member |

### Capabilities

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/capabilities` | List all capabilities |
| `GET` | `/capabilities/{id}` | Get a capability |
| `PUT` | `/capabilities/{id}` | Update a capability |

### Namespace folder items

```
GET /api/v1/namespace_folder/{id}/items?limit=50&offset=0&sort=defaultName&types=folder,report
```

| Param | Description |
|-------|-------------|
| `limit` | Max results |
| `offset` | Pagination offset |
| `sort` | Sort field |
| `types` | Comma-separated type filter |
| `fields` | Extra fields to return |

---

## 9. Security (API Keys)

API keys allow non-interactive authentication (no password required per request).

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/security/login_api_keys` | List all API keys |
| `POST` | `/security/login_api_keys` | Create a new API key |
| `GET` | `/security/login_api_keys/{id}` | Get an API key |
| `PUT` | `/security/login_api_keys/{id}` | Update an API key |
| `DELETE` | `/security/login_api_keys/{id}` | Delete an API key |

### Create an API key

```
POST /api/v1/security/login_api_keys
Content-Type: application/json
```

**Request body** (`LoginApiKeyPayload`):
```json
{
  "description": "CI/CD automation key",
  "expiry": "2025-12-31T00:00:00Z"
}
```

Returns a `LoginApiKeyPostResponse` that includes the raw key value — **store it now**, it will not be shown again.

---

## 10. Configuration (Namespaces & Keys)

### Authentication Namespaces

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/configuration/namespaces` | List namespace instances |
| `POST` | `/configuration/namespaces` | Create a namespace |
| `GET` | `/configuration/namespaces/{id}` | Get a namespace |
| `PUT` | `/configuration/namespaces/{id}` | Update a namespace |
| `DELETE` | `/configuration/namespaces/{id}` | Delete a namespace |
| `GET` | `/configuration/namespaces/test/{id}` | Test namespace connectivity |

### Global Configuration Keys

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/configuration/keys` | List all global keys |
| `POST` | `/configuration/keys` | Create one or more keys |
| `PUT` | `/configuration/keys` | Update one or more keys |
| `GET` | `/configuration/keys/{key}` | Get a specific key's value |
| `DELETE` | `/configuration/keys/{key}` | Delete / reset a key to its default |

### Data Governors

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/configuration/data_governors` | Get global data governors |
| `PUT` | `/configuration/data_governors` | Update global data governors |
| `POST` | `/configuration/data_governors?action={action}` | Perform an action on data governors |

---

## 11. Customizations, Themes & Extensions

### Customizations

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/customizations/{role}` | List customizations for a role |
| `PUT` | `/customizations/{role}` | Set customizations for a role |
| `GET` | `/perspectives` | List available perspectives |
| `GET` | `/system_profile_settings` | Get system profile settings |
| `PUT` | `/system_profile_settings` | Update system profile settings |
| `GET` | `/user_profile_settings` | Get default user profile |
| `PUT` | `/user_profile_settings` | Update default user profile |

### Themes

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/themes` | List all themes |
| `POST` | `/themes` | Add a theme (JSON definition) |
| `POST` | `/themes/file` | Upload a theme zip file |
| `DELETE` | `/themes/{name}` | Remove a theme |

### Extensions

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/extensions` | List all extensions |
| `POST` | `/extensions` | Add an extension |
| `POST` | `/extensions/file` | Upload an extension zip file |
| `DELETE` | `/extensions/{name}` | Remove an extension |

### Palettes

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/palettes/public` | List public palettes |
| `GET` | `/palettes/my` | List my (personal) palettes |
| `POST` | `/palettes/my` | Create a personal palette |
| `GET` | `/palettes/{id}` | Get a palette |
| `PUT` | `/palettes/{id}` | Update a palette |
| `DELETE` | `/palettes/{id}?forced=true` | Delete a palette |

---

## 12. Storage (Cloud Storage)

Cloud storage connections let Cognos read/write from S3-compatible or Azure Blob Storage.

### Connections

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/storage` | List all cloud storage connections |
| `GET` | `/storage/limited` | Get limited info for all connections |
| `POST` | `/storage` | Create a cloud storage connection |
| `GET` | `/storage/{id}` | Get a connection |
| `PUT` | `/storage/{id}` | Update a connection |
| `DELETE` | `/storage/{id}` | Delete a connection |
| `POST` | `/storage/test` | Test a connection |
| `GET` | `/storage/regions/{providerId}` | Get regions for a provider |

### Locations & Buckets

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/storage/{id}/buckets` | List buckets |
| `GET` | `/storage/{id}/locations` | List locations |
| `POST` | `/storage/{id}/locations` | Create a location |
| `GET` | `/storage/{id}/locations/limited` | List locations (limited info) |
| `GET` | `/storage/locations/{locationId}` | Get a location |
| `PUT` | `/storage/locations/{locationId}` | Update a location |
| `DELETE` | `/storage/locations/{locationId}` | Delete a location |
| `GET` | `/storage/locations/{locationId}/files` | List files at a location |
| `POST` | `/storage/locations/test` | Test a storage location |

---

## 13. Tenants

Multi-tenancy operations (Cognos Analytics multi-tenant deployments only).

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/tenants?tenant_id={id}` | List tenants or retrieve a specific one |
| `POST` | `/tenants?create_team_folder=true` | Create a tenant |
| `PUT` | `/tenants?tenant_id={id}` | Update a tenant |
| `DELETE` | `/tenants?tenant_id={id}` | Delete a tenant |
| `GET` | `/tenants/{id}` | Get tenant by store ID |
| `PUT` | `/tenants/set?tenant_id={id}` | Operate as a tenant member |
| `DELETE` | `/tenants/sessions?tenant_id={id}` | Terminate all sessions for a tenant |
| `DELETE` | `/tenants/profile?tenant_id={id}` | Delete tenant profile |
| `GET` | `/tenants/preferences?tenant_id={id}` | Get tenant preferences |
| `PUT` | `/tenants/preferences?tenant_id={id}` | Update tenant preferences |
| `GET` | `/tenants/user_profile_settings` | Get tenant user profile settings |
| `PUT` | `/tenants/user_profile_settings` | Update tenant user profile settings |
| `DELETE` | `/tenants/user_profile_settings` | Delete tenant user profile settings |
| `GET` | `/tenants/{id}/data_governors` | Get tenant data governors |
| `PUT` | `/tenants/{id}/data_governors` | Update tenant data governors |
| `POST` | `/tenants/{id}/data_governors?action={action}` | Perform action on tenant data governors |

---

## 14. Common Patterns & Tips

### Typical workflow to interact with a report

```
1. PUT  /api/v1/session            → authenticate, get session cookie
2. GET  /api/v1/content            → find the namespace root ID
3. GET  /api/v1/content/{id}/items → drill into folders to find the report ID
4. (use the Cognos Report Data Service separately for report execution)
5. DELETE /api/v1/session          → log out
```

### Pagination

Endpoints that return lists (`/groups`, `/namespace_folder/{id}/items`, etc.) support:

| Param | Type | Description |
|-------|------|-------------|
| `limit` | integer | Max records per page |
| `offset` | integer | Starting record index |
| `page` | integer | Page number (1-based, on some endpoints) |
| `sort` | string | Field to sort by |

### `fields` projection

Many GET endpoints accept a `fields` query parameter to reduce payload size:

```
GET /api/v1/content/{id}?fields=defaultName,type,ancestors,permissions
```

Common field names: `defaultName`, `type`, `ancestors`, `permissions`, `owner`, `created`, `modified`.

### Async task pattern

Endpoints that kick off long-running jobs (metadata import, file import) return a `taskid` and an `X-CA-Affinity` header.

```
POST /api/v1/.../import   →  { "taskid": "abc123" }
                              X-CA-Affinity: node01

# Poll with the affinity token:
GET  /api/v1/.../tasks/abc123
     X-CA-Affinity: node01

# Response status field:
{ "status": "running" | "complete" | "failed" | "cancelled" }
```

### Content types

All request and response bodies use `application/json` unless uploading binary files (zip / CSV), which use `multipart/form-data`.

### Error handling

| HTTP Code | Meaning |
|-----------|---------|
| `200` | OK |
| `201` | Created |
| `204` | No content (success, no body) |
| `400` | Bad request — check your JSON body |
| `401` | Not authenticated — call `PUT /session` first |
| `403` | Forbidden — namespace/permission issue |
| `404` | Object not found |
| `441` | Unauthorized (Cognos custom code, equivalent to 401 on some paths) |

### Quick reference: content store IDs

- **My Folders** (personal space): `i11D7FDC059B747CBB416A0E7FA8424F7`
- Find any object's ID by drilling: `GET /api/v1/content` → `GET /api/v1/content/{id}/items`

---

*Generated from the live Cognos Analytics 12.1.0 OpenAPI spec at `http://ibmdemos:9300/api/api-docs/swagger.json`.*
