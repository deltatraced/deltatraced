# 1 Journal

* [ ] 

From [^spawn-invst-0b60ed](004%20How%20is%20the%20game%20adding%20data%20to%20the%20end%20of%20the%20tape%3F.md#spawn-invst-0b60ed) in [3.9 Create a driver to search for death and infinity strings](004%20How%20is%20the%20game%20adding%20data%20to%20the%20end%20of%20the%20tape%3F.md#39-create-a-driver-to-search-for-death-and-infinity-strings)

2025-08-02 Wk 31 Sat - 01:40

When we press a key, like up and it registers, a new byte is added to the tape

````
/verify-tape
Warn: Expected tape length 13229 but got 13259
Warn: Expected tape sum 287251 but got NaN
/verify-tape
Warn: Expected tape length 13229 but got 13260
Warn: Expected tape sum 287251 but got NaN
````

We need to find out how.

### 1.1.1 Pend
