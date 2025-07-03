


#  Feasibility Assessment: Brevo Integration with Treasure Data CDP

  
**Goal:** Evaluate feasibility of integrating Brevo with Treasure Data CDP.

---

##  Scope

- Review Brevo’s API documentation  
- Identify potential integration risks and “danger zones”  
- Confirm whether a native integration exists  
- Highlight technical concerns or blockers  

---
## Review Brevo’s API documentation  


### Push to Brevo (e.g., contacts, events, campaigns) :

Pushing to Brevo is quite handy and supports different types of data, such as contacts, custom events, and email/SMS campaigns.
Data is sent to Brevo through its REST API by making secure HTTP requests to specific endpoints. An API key from the Brevo dashboard is required to authorize these requests. Contact data can be pushed to the /v3/contacts endpoint in JSON format, including fields such as email, first name, and list IDs. Events and campaigns are also supported through their respective endpoints, enabling the automation of marketing activities and the synchronization of user interactions. The integration supports real-time updates, ensuring that customer profiles and marketing workflows remain accurate and consistent

### Pull from Brevo (e.g., email stats, user info) : 



Data can be retrieved from **Brevo** to keep external systems updated with the latest marketing and contact information. This allows for integration with CRMs, data warehouses, and reporting tools. Brevo’s REST API is used to access various `GET` endpoints, through which data can be pulled.

#### Contacts

- `GET /v3/contacts` – A list of contacts can be retrieved with support for pagination and filtering.  
- `GET /v3/contacts/{email}` – Detailed information for a specific contact can be fetched.

#### Email Campaigns

- `GET /v3/emailCampaigns` – All created campaigns can be listed.  
- `GET /v3/emailCampaigns/{campaignId}` – Status, content, and performance metrics can be accessed.

#### Events

User interactions such as email opens and clicks are tracked by Brevo. These interactions can be:
- Accessed via **webhooks**  
- Exported through **transactional log endpoints** (available in selected plans)

#### Lists

- `GET /v3/contacts/lists` – Metadata about contact lists, including list IDs for segmentation, can be retrieved.

---

API keys are used for authentication, and responses are returned in **JSON** format.



### Brevo Authentication

Brevo offers two primary authentication methods for developers: **API Key Authentication** and **OAuth 2.0 Authentication**.

####  API Key Authentication

Brevo supports API key-based authentication, which is the fastest method to get started. To use it, generate an API key from the Brevo dashboard, then include it in the `api-key` header of your HTTP requests:
##### How It Is Used

1. An API key should be generated from the Brevo dashboard.
2. The key must be included in the `api-key` header of each request:

```http
GET /v3/contacts HTTP/1.1  
Host: api.brevo.com  
api-key: YOUR_API_KEY
 ```
- Implementation is **simplified** and can be completed quickly.  
- No **token exchanges** or **redirect flows** are required.  
- It is well-suited for **internal systems**, **automation**, and **backend jobs**.

---

#####  Risks That Should Be Considered

- API keys are **not automatically expired** unless rotated manually.  
- **Full access** is often granted without scope restrictions.  
- If the key is **exposed** (e.g., in frontend code or repositories), **misuse** can occur.

####  OAUTH Authentication


Brevo offers OAuth 2.0 authentication. It provides better security and access control. OAuth allows you to request specific scopes and handle token expiration through refresh tokens.

#### How It Works

- An application must be registered with Brevo to be issued a **client ID** and **client secret**.  
- Users are redirected to Brevo’s **authorization URL**.  
- An **authorization code** is returned via the configured **redirect URI**.  
- The code is then exchanged for an **access token**.  
- The access token is included in the API call using the `Authorization: Bearer` header:

```http
GET /v3/contacts HTTP/1.1  
Host: api.brevo.com  
Authorization: Bearer ACCESS_TOKEN

```

#### Advantages That Are Provided

- Access is restricted using **scopes**, allowing only the necessary permissions.  
- Tokens can be **revoked** or **set to expire automatically**, enhancing control.  
- This method is preferred when **third-party applications** are integrated or **multiple user accounts** are managed.

---

#### Downsides That Might Be Faced

- A **higher setup effort** is required, including configuration of **redirect URIs** and **token exchange logic**.  
- **Secure storage** of tokens and **refresh logic** must be implemented and maintained.

#### API Rate Limits



Brevo applies rate limits to its API in order to maintain system performance and ensure fair use among all users. Under the Enterprise plan, these limits are much higher to support large-scale operations. For instance, the `POST /v3/smtp/email` and `GET /v3/smtp/blockedContacts` endpoints allow up to **7,200,000 requests per hour** or **2,000 per second**. The `GET /v3/smtp/emails` endpoint is limited to **10,800 requests per hour** or **3 per second**. For sending SMS using `POST /v3/transactionalSMS/sms`, the limit is **720,000 per hour** or **200 per second**. Event data can be sent through `POST /v3/events` at **72,000 per hour** or **20 per second**. Other `/v3/smtp/...` endpoints are limited to **600 requests per hour**, and `/v3/contacts/...` endpoints allow up to **72,000 per hour** or **20 per second**. Any other API endpoints have a default limit of **200 requests per hour**. These limits help protect the system from overuse and ensure reliable service for all clients.

  #### Brevo Batch API Support

Support for sending multiple records in a single API call is provided by Brevo through its batch endpoints. For example, when transactional emails are sent, the `messageVersions` field can be used to include up to **1,000 personalized messages** in one request. This allows large-scale communications to be handled efficiently.

Similarly, batch endpoints are provided for the bulk addition or update of contacts, as well as for managing e-commerce data such as **orders** and **products**. Through these batch operations, API performance can be optimized, the number of individual requests can be reduced, and compliance with rate limits can be maintained during high-volume processing.


#  Technical Concerns and Blockers  
**Brevo Integration with Treasure Data CDP**

When planning a custom integration between Brevo and Treasure Data, several technical concerns and potential blockers should be taken into account:

### 1.  Authentication and Authorization
- **Brevo uses API key authentication**, which lacks fine-grained scopes and expiration control.
- **Token mismanagement** could pose security risks if API keys are not stored or rotated securely.

### 2.  API Rate Limits
- Brevo enforces **strict rate limits** based on endpoint types (e.g., 2,000 requests/second for SMTP, 20/second for events, 600/hour for some SMTP endpoints).
- High-frequency sync jobs from Treasure Data may hit rate limits without proper throttling and queuing logic.

### 3.  Data Mapping and Schema Alignment
- **Field names and structures** may differ significantly between Treasure Data and Brevo (e.g., contact properties, event formats).
- Complex field mappings and transformation logic may be required to avoid data loss or rejection errors.

### 4.  Batch Limitations
- Brevo supports batch operations, but **message and contact imports are capped at 1,000 records per request**.
- Without proper batching logic, larger datasets may fail or be only partially synced.


---



##  Summary

There is **no official/native integration** between Brevo and Treasure Data.  
A **custom integration is technically feasible** using Brevo’s REST API and Treasure Data’s ingestion/export tools (REST API or TD Toolbelt).  
While the integration is technically doable, careful planning is required to address these risks.

---

##  References

- [ Brevo API Docs](https://developers.brevo.com/)
- [ Brevo Developer Hub](https://developers.brevo.com/reference/getting-started-1)
- [ Treasure Data REST API](https://api-docs.treasuredata.com/)


---
**Author:** Errafay Amine

