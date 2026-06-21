<div align="center">

# Quickbooks Ruby API Client

Ruby library for working with the QuickBooks Online REST API v3.

</div>

## 📌 Overview

`quickbooks-ruby` provides Ruby integration with QuickBooks Online through the Intuit Data Services v3 REST API. It is designed for applications that need to authenticate with Intuit, access QuickBooks Online company data, query supported entities, and perform accounting-related operations through Ruby service objects and models.

The library communicates with the QuickBooks Online accounting API and includes support for common workflows such as retrieving customers, creating invoices, updating records, deleting supported entities, batching requests, working with attachments, downloading transaction PDFs, querying reports, and using Change Data Capture.

The QuickBooks Online API is documented by Intuit at: https://developer.intuit.com/docs/api/accounting

## 🧩 Technology

| Area | Details |
| --- | --- |
| Language | Ruby |
| API | QuickBooks Online REST API v3 |
| Authentication | OAuth 2.0 |
| Serialization | XML, with JSON support noted for selected services |
| Key dependencies | `oauth2`, `roxml`, `nokogiri`, `active_model` |

## ⚠️ Ruby Version Notes

Version 2 of `quickbooks-ruby` was released on Nov 10, 2022 and no longer supports Ruby 2.5.

| quickbooks-ruby version | Branch | Ruby |
| --- | --- | --- |
| 1 | `master` | `<= 2.5` |
| 2 | `2-stable` | `>= 2.6.0` |

Ruby 1.9.x is not supported.

## 📦 Installation

Add the gem to your application using the package name:

`quickbooks-ruby`

For Bundler-based applications, include it in your Gemfile as:

`gem 'quickbooks-ruby'`

The original documentation also describes installing the gem directly with RubyGems using the same package name.

## 🔐 Authentication

The library uses OAuth 2.0 for authentication with Intuit. A typical integration creates an OAuth client using Intuit’s authorization and token endpoints, redirects the user through Intuit’s authorization flow, receives the callback, and stores the resulting access token, refresh token, and realm ID.

Important authentication details from the project documentation:

- Access tokens are valid for one hour.
- Refresh tokens should be persisted and refreshed before or after token expiration.
- Expired or unauthorized access may raise `OAuth2::Error`.
- The QuickBooks company ID is also known as the Realm ID.
- Applications usually persist OAuth credentials so users do not need to authorize every session.

## 🧪 Sandbox Mode

By default, the gem runs in production mode. QuickBooks Online sandbox access can be enabled for development or testing:

`Quickbooks.sandbox_mode = true`

The sandbox endpoint documented by the project is:

`https://sandbox-quickbooks.api.intuit.com`

## 🔢 Minor Version

As of July 2025, minor version `75` is the minimum supported version documented by the project. If not specified, the QuickBooks Online API defaults to this version.

A custom minor version can be configured with:

`Quickbooks.minorversion = 75`

## 🧾 Core Usage Areas

`quickbooks-ruby` organizes API interaction around service objects and model objects. A service is configured with the QuickBooks company ID and an OAuth access token, then used to query or mutate supported QuickBooks entities.

Documented capabilities include:

- Querying entities with QuickBooks Online’s SQL-like query language
- Fetching a single entity by ID
- Finding objects by matching attributes
- Retrieving all objects for supported services
- Creating invoices, sales receipts, customers, items, and other supported entities
- Updating records, including sparse updates
- Deleting supported records
- Sending invoices by email
- Performing batch operations with up to 25 objects
- Uploading attachments or attachment metadata
- Downloading PDFs for invoices, sales receipts, or payments
- Accessing Change Data Capture data
- Querying supported QuickBooks reports

## 🔎 Querying Data

The library supports standard QuickBooks Online query operations. A service can issue the default query for an entity or accept a complete SQL-like QuickBooks query string.

Pagination is handled through options such as `page` and `per_page` rather than embedding pagination parameters directly in the query string.

For larger result sets, the project documents a `query_in_batches` collection method that can iterate over batches of records. The default batch option is `per_page: 1000`.

A basic query builder is also included to help escape complex query clauses in the format required by Intuit.

## ✏️ Updating Records

The documentation notes an important behavior for updates: updates are not sparse by default. If an update request omits attributes, those attributes may be unset by QuickBooks Online.

Sparse updates are supported for cases where only selected fields should be changed. This is useful when the full object is not available and the integration only needs to update a small set of attributes.

## 📄 Invoices and Sales Receipts

The library includes examples and support for creating invoices and sales receipts using QuickBooks models and service classes.

Documented invoice-related workflows include:

- Creating an invoice for a customer
- Adding line items
- Setting transaction dates and document numbers
- Creating invoices that include bundle line items
- Sending invoices by email
- Downloading invoice PDFs

For sales receipts, the documentation describes creating a receipt with customer, payment, deposit account, payment method, and line item information.

## 📎 Attachments and PDFs

QuickBooks Online supports attachment metadata and file uploads. The project documents separate services for metadata-only attachment records and actual file uploads.

The library also supports downloading raw PDF data for invoices, sales receipts, and payments through the relevant service.

## 🔄 Change Data Capture

QuickBooks Online Change Data Capture allows applications to discover recently changed entities. The documented API supports requesting changes up to 30 days ago.

The project includes `Quickbooks::Service::ChangeDataCapture` and related change services for entities such as invoices, customers, vendors, items, payments, purchases, and credit memos.

Deleted entities can be identified in Change Data Capture responses by checking whether their status is `Deleted`.

## 📊 Reports API

The library includes support for the QuickBooks Online Reports API, which provides access to accounting reports such as business summaries, sales views, vendor balances, customer balances, expenses, purchases, and related reporting data.

## 🧱 Supported Entity Coverage

The README documents implementation coverage for many QuickBooks Online entities, including:

- Account
- Bill
- Bill Payment
- Class
- Company Info
- Credit Memo
- Customer
- Department
- Deposit
- Employee
- Estimate
- Invoice
- Item
- Journal Entry
- Payment
- Payment Method
- Preferences
- Purchase
- Purchase Order
- Refund Receipt
- Sales Receipt
- Tax Agency
- Tax Code
- Tax Rate
- Tax Service
- Term
- Time Activity
- Vendor
- Vendor Credit

Support varies by entity and operation. The documented operation categories include create, update, query, delete, fetch by ID, and other entity-specific behavior.

## 🧰 Logging and Debugging

Logging can be enabled globally:

`Quickbooks.log = true`

Logging can also be configured per service instance. By default, logging is directed to standard output, but a custom logger such as a Rails logger can be assigned.

The documentation also describes HTTP proxy configuration for debugging API traffic with tools such as Charles Proxy.

## ❓ FAQ

### What is quickbooks-ruby used for?

`quickbooks-ruby` is used to connect Ruby applications to QuickBooks Online through Intuit’s REST API v3. It helps Ruby applications authenticate with Intuit and work with QuickBooks accounting entities.

### Does it support OAuth 2.0?

Yes. The documentation describes OAuth 2.0 authentication using Intuit authorization and token endpoints, including token refresh handling.

### Does it support sandbox testing?

Yes. Sandbox mode can be enabled with `Quickbooks.sandbox_mode = true`.

### Does it support JSON?

The project notes that the QuickBooks v3 API started with XML and JSON support, while some newer services such as Tax Service are JSON-only. Full JSON support is listed as a TODO in the original documentation.

### Are sparse updates supported?

Yes. Sparse updates are supported by passing the sparse update option when updating an object.

### Can it send invoices by email?

Yes. The documented invoice service includes a send invoice feature that can send to the invoice billing email or an alternate email address.

## 📜 License

This project is licensed under the MIT License.
