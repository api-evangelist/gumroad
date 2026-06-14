# Gumroad GraphQL Schema

## Overview

This document describes a conceptual GraphQL schema for the Gumroad digital commerce and creator economy platform. Gumroad enables creators to sell digital products, memberships, courses, software, music, and physical goods directly to their audience. The schema is derived from the Gumroad v2 REST API (https://app.gumroad.com/api) and models the full surface of the platform including products, sales, subscribers, license keys, offer codes, affiliates, webhooks, and creator accounts.

Authentication uses OAuth 2.0 access tokens passed as Bearer tokens. The conceptual GraphQL layer maps directly onto the REST endpoints documented at https://app.gumroad.com/api.

## Schema Source

- REST API Documentation: https://app.gumroad.com/api
- OAuth Applications: https://gumroad.com/oauth/applications
- Base URL: https://api.gumroad.com/v2

## Types

### Product Domain

- **Product** — A digital or physical item listed for sale on Gumroad, including pricing, content type, and publication status.
- **ProductDetails** — Extended metadata for a product including description, cover image, preview URL, and tags.
- **ProductType** — Enumeration of product kinds: digital, physical, subscription, bundle.
- **ProductStatus** — Enumeration of product states: published, unpublished, deleted.
- **ProductPrice** — Pricing structure for a product including amount, currency, and pay-what-you-want settings.
- **ProductContent** — The downloadable or streamable content attached to a product.
- **DigitalFile** — A file asset attached to a product, with download URL and metadata.
- **FileDetails** — Detailed metadata for a digital file including name, size, mime type, and upload date.
- **Variant** — A named grouping of options for a product (e.g., "Format", "Size").
- **VariantDetails** — Extended metadata for a variant including description and display order.
- **VariantOption** — A single selectable option within a variant (e.g., "PDF", "Large").
- **VariantOptionDetails** — Pricing delta and availability data for a variant option.

### Offer and Discount Domain

- **OfferCode** — A discount code applicable to one or more products.
- **OfferCodeDetails** — Extended data for an offer code including redemption count and validity window.
- **DiscountType** — Enumeration of discount kinds: percent, cents.

### Custom Fields Domain

- **CustomField** — A creator-defined field shown at checkout to collect buyer information.
- **CustomFieldType** — Enumeration of custom field data types: text, checkbox, terms.

### Purchase Domain

- **Purchase** — A completed transaction in which a customer acquires a product.
- **PurchaseDetails** — Extended metadata for a purchase including email, IP, referrer, and custom field responses.
- **PurchaseStatus** — Enumeration of purchase states: successful, failed, refunded, chargedback.

### License Domain

- **License** — A license record tied to a purchase, governing product usage rights.
- **LicenseDetails** — Extended data for a license including activation count and max activations.
- **LicenseType** — Enumeration of license models: single, multi-seat, site.
- **LicenseKey** — The alphanumeric key string associated with a license.

### Subscriber and Subscription Domain

- **Subscriber** — A customer enrolled in a recurring membership or subscription product.
- **SubscriberDetails** — Extended data for a subscriber including billing dates and failed charge history.
- **SubscriberStatus** — Enumeration of subscriber states: alive, pending_cancellation, pending_failure, failed_payment, fixed_subscription_period_ended, cancelled.
- **Subscription** — A recurring billing arrangement attached to a membership product.
- **SubscriptionDetails** — Billing cycle, amount, and renewal date for a subscription.

### Membership Domain

- **Membership** — A product type offering recurring access to content or community.
- **MembershipDetails** — Extended metadata for a membership including benefit description and access URL.
- **MembershipTier** — A level within a membership product (e.g., "Basic", "Pro").
- **MembershipPlan** — Billing plan associated with a membership tier including interval and price.

### Post Domain

- **Post** — A creator-published content post delivered to subscribers or the public.
- **PostDetails** — Extended metadata for a post including body HTML, published date, and audience filter.
- **PostType** — Enumeration of post kinds: published, draft, scheduled.

### Sales and Financial Domain

- **Sale** — A record of product revenue from a completed purchase.
- **SaleDetails** — Breakdown of a sale including price, taxes, fees, and affiliate commission.
- **SaleAmount** — Monetary value breakdown of a sale: gross, net, gumroad fee, and payout amount.
- **Refund** — A reversal of a purchase transaction.
- **RefundDetails** — Extended data for a refund including reason, refunded amount, and date.
- **Payout** — A disbursement of earnings from Gumroad to the creator's bank account.
- **PayoutDetails** — Extended data for a payout including bank, amount, date, and transaction reference.

### Creator and User Domain

- **Creator** — The authenticated Gumroad user account (seller).
- **CreatorDetails** — Extended profile data for a creator including bio, social links, and payout settings.

### Customer and Follower Domain

- **Customer** — A buyer who has purchased one or more products from a creator.
- **CustomerDetails** — Extended data for a customer including purchase history and email.
- **Follower** — A user who follows a creator without necessarily having made a purchase.
- **FollowerDetails** — Extended data for a follower including follow date and email.

### Auth and Webhook Domain

- **APIKey** — An OAuth application credential used to authenticate API requests.
- **Token** — An OAuth 2.0 access or refresh token.
- **Webhook** — A resource subscription that delivers event notifications to a URL.
- **WebhookEvent** — An event payload delivered to a registered webhook endpoint.

## Queries

- `products` — List all products for the authenticated creator.
- `product(id: ID!)` — Retrieve a single product by ID.
- `sales` — List all sales, filterable by date range and product.
- `sale(id: ID!)` — Retrieve a single sale by ID.
- `subscribers(productId: ID!)` — List subscribers for a membership product.
- `subscriber(id: ID!)` — Retrieve a single subscriber by ID.
- `offerCodes(productId: ID!)` — List offer codes for a product.
- `offerCode(id: ID!, productId: ID!)` — Retrieve a single offer code.
- `customFields(linkId: ID!)` — List custom fields for a product.
- `license(productPermalink: String!, licenseKey: String!)` — Retrieve a license by key.
- `user` — Retrieve the authenticated creator's account.
- `resourceSubscriptions` — List all registered webhooks.

## Mutations

- `createProduct` — Create a new product.
- `updateProduct` — Update an existing product.
- `deleteProduct` — Delete a product.
- `enableProduct` — Enable (publish) a product.
- `disableProduct` — Disable (unpublish) a product.
- `createVariant` — Add a variant to a product.
- `updateVariant` — Update a product variant.
- `deleteVariant` — Delete a product variant.
- `createOfferCode` — Create a new offer code.
- `updateOfferCode` — Update an offer code.
- `deleteOfferCode` — Delete an offer code.
- `createCustomField` — Add a custom field to a product.
- `updateCustomField` — Update a custom field.
- `deleteCustomField` — Delete a custom field.
- `enableLicense` — Enable a license key.
- `disableLicense` — Disable a license key.
- `decrementLicenseUsesCount` — Decrement the activation count for a license.
- `subscriberUnsubscribe` — Unsubscribe a subscriber.
- `refundSale` — Issue a refund for a sale.
- `createWebhook` — Register a new resource subscription (webhook).
- `updateWebhook` — Update a webhook URL or resource name.
- `deleteWebhook` — Remove a webhook registration.
