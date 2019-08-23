# Customer Identity Management

Knowing who your customers are and what they are doing on your app is critical to proper engagement. Zaius' SDK lets you manage that identity. More information about the technical aspects of this can be found in the [Customer API reference](./).

### When to Identify

When a Customer signs into your app, it appears to Zaius to be a new Customer. In order to make sure that the Customer is properly associated with all of their existing data, you should use [the `Zaius.identify()` function](./#updating-customer-identifiers). This will let you associate a name or email address with the internal tracking info to get a more complete picture of who is using the app.

### Internally, VUIDs

One of the ways Zaius maintains a customer's identity through various email, name, phone number, etc changes is by using a unique identifier that is sent with all requests. This is called the `VUID` and isn't normally visible to end users.

### When to stop identifying

If the customer ever signs out of the app \(for example, if they want to use a different account\), you will want to make sure the new user stays separate from previous data. You can separate this information with `Zaius.anonymize()` when they sign out. This will regenerate a new VUID for them and effectively remove the link between any new events they may generate and any customer information they had previously stored.

