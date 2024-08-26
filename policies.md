In RabbitMQ, policies are a powerful feature that allows you to apply various settings to queues, exchanges, and other objects within the messaging system. These policies help manage behavior related to availability, reliability, and performance. Here's an explanation for each option listed in your question, along with examples:

### Queues [All types]

1. **Max length**: Limits the number of messages a queue can hold. Once the limit is reached, the queue will either drop or reject new messages depending on the overflow behavior setting.
   - **Example**: Setting `max-length` to `5000` means the queue can hold a maximum of 5000 messages.

2. **Max length bytes**: Limits the total size of messages (in bytes) that a queue can hold.
   - **Example**: Setting `max-length-bytes` to `104857600` (100MB) limits the total size of all messages in the queue to 100MB.

3. **Overflow behavior**: Determines what happens when a queue reaches its `max-length` or `max-length-bytes` limit. Options include `drop-head` (oldest messages are dropped), `reject-publish` (new messages are rejected), and others.
   - **Example**: Setting `overflow` to `drop-head` will start dropping the oldest messages when the queue is full.

4. **Auto expire**: Sets the time in milliseconds after which a queue will be automatically deleted if it's not used (no consumers, no new messages).
   - **Example**: Setting `auto-expire` to `3600000` will delete the queue if it's inactive for an hour.

5. **Dead letter exchange**: Specifies an exchange to which messages from the queue will be republished if they are rejected or expire.
   - **Example**: Setting `dead-letter-exchange` to `my-dlx` sends dead-lettered messages to the `my-dlx` exchange.

6. **Dead letter routing key**: Sets the routing key to be used when dead-lettered messages are published to the specified dead letter exchange.
   - **Example**: Setting `dead-letter-routing-key` to `dlx-routing` routes dead-lettered messages using this specific key.

### Queues [Classic]

1. **HA mode**: Configures the high availability mode for classic queues, such as `all` (mirrors across all nodes), `exactly` (mirrors on a specific number of nodes), or `nodes` (mirrors on specific nodes).
   - **Example**: Setting `ha-mode` to `all` ensures that all nodes in the cluster keep a copy of the queue.

2. **HA params**: Parameters specific to the selected HA mode, like count of nodes for the `exactly` mode.
   - **Example**: If `ha-mode` is `exactly`, `ha-params` could be set to `2` to indicate two mirrors.

3. **HA sync mode**: Determines the synchronization mode for mirroring queues (`automatic` or `manual`).
   - **Example**: Setting `ha-sync-mode` to `automatic` ensures that queue mirrors are automatically kept in sync.

4. **HA mirror promotion on shutdown**: Controls if a mirror can be promoted to a master when the current master is shut down gracefully.
   - **Example**: Setting this to `on-shutdown` promotes a mirror to master if the master node is shut down.

5. **HA mirror promotion on failure**: Controls if a mirror can be promoted to a master when the current master fails.
   - **Example**: Setting this to `always` ensures a mirror is promoted to master upon any failure of the current master.

6. **Message TTL**: The time-to-live for messages in the queue, in milliseconds.
   - **Example**: Setting `message-ttl` to `60000` means messages will expire after one minute if not consumed.

7. **Lazy mode**: Queues in lazy mode move messages to disk as soon as possible, providing more predictable performance under load.
   - **Example**: Setting `lazy-mode` to `lazy` helps manage memory better by storing messages on disk.

8. **Master Locator**: Determines how the master node is selected when using mirrored queues.
   - **Example**: Setting `master-locator` to `min-masters` selects the node with the fewest masters.

### Queues [Quorum]

1. **Max in memory length**: Limits the number of messages that can be stored in memory before they must be written to disk.
   - **Example**: Setting `max-in-memory-length` to `1000` limits the in-memory queue size to 1000 messages.

2. **Max in memory bytes**: Similar to `max in memory length`, but limits the total bytes used in memory.
   - **Example**: Setting `max-in-memory-bytes` to `50000000` (50MB).

3. **Delivery limit**: The maximum number of unacknowledged messages that can be delivered to consumers.
   - **Example**: Setting `delivery-limit` to `200` limits the number of in-flight messages.

### Queues [Stream]

1. **Max age**: The maximum age (in milliseconds) of any message in the stream queue before it's automatically deleted.
   - **Example**: Setting `max-age` to `86400000` would delete messages older than a day.

2. **Max segment size in bytes**: Specifies the maximum size of each log segment file in a stream queue.
   - **Example**: Setting `max-segment-size-in-bytes` to `104857600` (100MB) limits segment file size to 100MB.

### Exchanges

1. **Alternate exchange**: An exchange to which messages are routed if they cannot be routed to any queue.
   - **Example**: Setting `alternate-exchange` to `my-alt-exchange`.

### Federation

1. **Federation upstream set**: Configures a set of upstream brokers for federation.
   - **Example**: Setting `federation-upstream-set` to `upstream-set1`.

2. **Federation upstream**: Specifies a single upstream broker for federation.
   - **Example**: Setting `federation-upstream` to `my-upstream-broker`.

These policy settings allow for extensive customization and optimization of RabbitMQ's behavior, tailored to specific application requirements and environments.


### RabbitMQ policy options for different queue types, including numeric value ranges where applicable:

### For All Queue Types:
1. **Max length**: Sets the maximum number of messages the queue can hold. If not specified, there is no limit.
2. **Max length bytes**: Specifies the maximum total size of all messages in bytes that a queue can hold.
3. **Overflow behavior**: Configures what happens when the queue reaches its max length or max length bytes. Options include `drop-head` (oldest messages are dropped), `reject-publish` (new messages are rejected), or `reject-publish-dlx` (messages are rejected and sent to a dead letter exchange).
4. **Dead letter exchange**: Designates an exchange to which messages will be redirected if they are dead-lettered.
5. **Dead letter routing key**: Specifies the routing key used when dead-lettered messages are published to the dead letter exchange.
6. **Auto expire**: Sets the duration (in milliseconds) after which the queue will be deleted if it is not used.

### For Classic Queues:
1. **HA mode**: Controls the high availability settings, with modes like `all`, `exactly`, or `nodes`.
2. **HA params**: Provides parameters for the selected HA mode, such as the number of nodes.
3. **HA sync mode**: Determines whether the queue mirrors are synchronized automatically or manually.
4. **Message TTL**: Defines the time-to-live for messages in milliseconds.
5. **Lazy mode**: Moves messages to disk as soon as possible to reduce memory usage.
6. **Master locator**: Sets the rule for selecting the master node in mirrored setups.

### For Quorum Queues:
1. **Max in-memory length**: Limits the number of messages stored in memory before moving to disk.
2. **Max in-memory bytes**: Limits the total message size in memory in bytes.
3. **Delivery limit**: Restricts the number of unacknowledged messages delivered to consumers.

### For Stream Queues:
1. **Max age**: Sets the maximum age of any message in the stream before it is automatically deleted.
2. **Max segment size in bytes**: Specifies the maximum size of each segment file in the stream.

### General Options for Exchanges:
1. **Alternate exchange**: Specifies an alternate exchange where messages will be routed if they cannot be delivered to any queue.

### For Federation:
1. **Federation upstream set**: Configures a set of upstream brokers.
2. **Federation upstream**: Specifies a single upstream broker.

These options provide extensive control over how messages are handled in RabbitMQ, allowing configurations for performance optimizations, high availability, and more reliable message handling across different scenarios. For specific numeric ranges and more in-depth configuration details, you can refer directly to the RabbitMQ official documentation [here](https://www.rabbitmq.com).
