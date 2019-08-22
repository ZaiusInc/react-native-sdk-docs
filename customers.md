# Customers

Customers are central to Zaius' operation. You can find out more about how they are managed at the [Zaius Documentation](https://developer.zaius.com/core-concepts/overview/customers).

### Updating Customer Information

In order for Zaius to operate, it's important to make sure that it is operating with the most relevant information it can. You can use `Zaius.customer()` to make sure you have good information about a Customer.

```text
Zaius.customer(
    {
        ... // identifiers
    },
    {
        ... // attributes
    }
)
```

Identifiers will be explained below.

Attributes are bits of information about the customer, like first/last name, phone number, gender, etc.

### Updating Customer Identifiers

Zaius will use all information available to it in order to coalesce events into a single customer record when possible. This process can be aided by adding identifying information directly. This is done with `Zaius.identify()`

```text
Zaius.identify({
   ...
})
```

The content of the document is fairly freeform, but will automatically have the VUID generated for this user included. This document can include other fields like email address that are more fully documented in the [Zaius Customer Documentation](https://developer.zaius.com/core-concepts/overview/customers).

This function call is effectively the same as `Zaius.customer({...}, {})`

### VUIDs

One of the ways Zaius maintains a customer's identity through various email, name, phone number, etc changes is by using a unique identifier that is sent with all requests. This is called the `VUID` and isn't normally visible to end users.

However, if the customer ever wants to become anonymous and separate from previous data, they can anonymize their information with `Zaius.anonymize()`. This will regenerate a new VUID for them and effectively remove the link between any new events they may generate and any customer information they had previously stored.
