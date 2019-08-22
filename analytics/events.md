# Events

Events are the main way that an app communicates with Zaius. You can find more about Events at the [Zaius Documentation](https://developer.zaius.com/core-concepts/overview/events).

### Sending Events

The main way of sending events to Zaius is with `Zaius.event()`.

```typescript
Zaius.event({
    type: string,
    action: string,
    identifiers: {
        ...
    },
    data: {
        ...
    },
})
```

* `type`: The type of event. You can think of it as what the event is happening about. You can find more about this in the [Zaius Event Overview](https://developer.zaius.com/core-concepts/overview/events).
* `action`: What the customer is doing that Zaius should be told about.
* `identifiers`: Various bits of information that can identify a Customer. This will include things like the [VUID](customers.md), push token, or email. Values in this object should be strings.
* `data`: Relevant data to the event. For example, the event sent when Push Notifications permission is granted will also contain the push token for the phone \(so Zaius can actually deliver notifications\). The values here should be strings.

### Event Queue

Each time an event is sent to Zaius via `Zaius.event()` or automatically, it gets enqueued into the Event Queue -- the event is **not** sent immediately.

The Event Queue holds on to the pending events. It is flushed out every 30 seconds and all pending requests are made to Zaius. Each time a new event is added, the queue state is peristed to survive app quits and crashes. Queueing this way allows for more efficient network communication in a way that does not interrupt your customer's flow.

If the `startQueue` option to `Zaius.configure()` is `true`, the queue will start processing events as soon as it can load them from the phone's storage.

If `startQueue` is `false`, then the Queue must be started manually by calling `z.queue.start()` \(where `z` is the return value of `Zaius.configure()`\), but setting this to `false` is **NOT** recommended.

### Automatic Events

Some events are events that the SDK automatically sends out for you. They are:

* Add Push Token: This event is sent when the phone finishes registering for Push Notifications. If this event gets sent, then the phone is capable of recieving pushes.
* Push Notification Open: This event is sent when the user taps on a push notification. Note: on Android, the notification will always show. On iOS, the notification will only show if the app is in the background.

