# Simple Gift Card

This simple application demonstrates the CQRS (Command Query Responsibility Segregation) and Event Sourcing patterns using the Axon Java Framework. The domain used is gift cards. This application is demoed during this [presentation](https://speakerdeck.com/reza_rahman/what-is-cqrs-plus-event-sourcing-and-why-should-java-developers-care). [Gift cards](/src/main/java/io/axoniq/giftcard/command/GiftCard.java) themselves are event sourced aggregate entities that constitute the write model, execute commands and emit events. The [read model](/src/main/java/io/axoniq/giftcard/query) handles events, constructs several materialized views and powers several queries.   

The Maven based application only requires Java SE 8 (or up). You should be able to open the application in any Maven capable IDE. Once built, you can run the application through the following command.

```
java -jar gift-card-simple-1.0.jar
```

Once the application is up and running, you can access it via http://localhost:8080.

![UI](/screenshots/ui.jpg)

Through the UI, you can issue single cards, bulk issue cards, redeem cards, and view a list of cards. You can also see what is going on in the database (including the event store) by navigating to http://localhost:8080/h2. When prompted, just enter the following as the JDBC URL (accept all the other defaults).

```
jdbc:h2:mem:testdb
```
![UI](/screenshots/h2.jpg)

Once logged in, you can see all triggered events stored in the event store by issuing the following SQL:

```sql
SELECT
    GLOBAL_INDEX,
    EVENT_IDENTIFIER,
    UTF8TOSTRING(META_DATA) AS META_DATA,
    UTF8TOSTRING(PAYLOAD) AS PAYLOAD,
    PAYLOAD_TYPE,
    TIME_STAMP,
    AGGREGATE_IDENTIFIER,
    SEQUENCE_NUMBER,
    TYPE
FROM
    DOMAIN_EVENT_ENTRY
```
