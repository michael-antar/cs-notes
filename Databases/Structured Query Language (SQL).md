## Indexes
### Clustered
### Non-Clustered
#### Covering
Special case where Non-Clustered indexing is faster and requires no Hops.
### Comparison

| Feature        | Clustered Index                        | Non-Clustered Index                           |
| -------------- | -------------------------------------- | --------------------------------------------- |
| Quantity       | Max **1** per table                    | Unlimited (technically 999+)                  |
| Physical Order | **Yes**. Rows are stored in this order | **No**. Stored separately                     |
| Data Storage   | The leaf node _is_ the Data Row        | The leaf node is a _Pointer_ to the Data Row  |
| Best For       | Ranges, Sorting, Primary Keys          | Looking up single values (Emails, Usernames)  |
| Performance    | Fastest (Zero Hops)                    | Fast (One Hop), unless "Covering" (Zero Hops) |
