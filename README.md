# First Django Tenants App Using Django Tenant
First Django  tenants app using `django-tenant` 

## Welcome to django-tenants documentation!

This application enables Django-powered websites to have multiple tenants via PostgreSQL schemas. A vital feature for every Software-as-a-Service website.

Django currently provides no simple way to support multiple tenants using the same project instance, even when only the data is different. Because we don’t want you running many copies of your project, you’ll be able to have:

Multiple customers running on the same instance

Shared and Tenant-Specific data

Tenant View-Routing

## What are schemas?
A schema can be seen as a directory in an operating system, each directory (schema) with its own set of files (tables and objects). This allows the same table name and objects to be used in different schemas without conflict. For an accurate description of schemas, see PostgreSQL’s official documentation on schemas.

## Why schemas?
There are typically three solutions for solving the multitenancy problem.

- Isolated Approach: Separate Databases. Each tenant has its own database.

- Semi-Isolated Approach: Shared Database, Separate Schemas. One database for all tenants, but one schema per tenant.

- Shared Approach: Shared Database, Shared Schema. All tenants share the same database and schema. There is a main tenant table, whereas all other tables have a foreign key pointing.

This application implements the second approach, which in our opinion, represents the ideal compromise between simplicity and performance.

Simplicity: barely make any changes to your current code to support multitenancy. Plus, you only manage one database.

Performance: make use of shared connections, buffers, and memory.

Each solution has it's up and downsides, for a more in-depth discussion, see Microsoft’s excellent article on Multi-Tenant Data Architecture.

## How it works
Tenants are identified via their hostname (i.e tenant.domain.com). This information is stored in a table on the public schema. Whenever a request is made, the hostname is used to match a tenant in the database. If there’s a match, the search path is updated to use this tenant’s schema. So from now on, all queries will take place at the tenant’s schema. For example, suppose you have a tenant customer at http://customer.example.com. Any request incoming at customer.example.com will automatically use the customer’s schema and make the tenant available at the request. If no tenant is found, a 404 error is raised. This also means you should have a tenant for your main domain, typically using the public schema. For more information please read the <a href='https://django-tenants.readthedocs.io/en/latest/'>django-tenants.readthedocs.io/</a>.
