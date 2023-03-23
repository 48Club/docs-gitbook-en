# Score

The score mechanism has been introduced to encourage effective usage of the puissant service.&#x20;

Each sender has its own score, which is calculated and applied independently. The reputation is a floating-point number with an initial value of 10.&#x20;

Each time a puissant is successfully packaged, the score increases by the value of:

$$
3*min(10,\frac{gas\_fee}{21000*60gwei})
$$

where the gas\_fee means the actual gas consumed by the first tx in puissant, and 60 gwei in the formula is a configurable item, i.e., the puissant gas\_price\_floor, which can be checked [here](puissant-api.md#query-gas-price-floor).

When a puissant fails to package due to any tx revert, the score **decreases by 1**.

Every unit time (hour), the score of all senders from each IP will be accumulated in the past 24-hour time window. The value from the last snapshot can be queried through [the query endpoint](puissant-api.md#query-puissant-reputation), if there is no record from the last time, the initial value is returned.

If the score value of IP is **negative**, puissant requests from which will be **refused** by the puissant service in the next unit time, and a low score error code will be returned.
