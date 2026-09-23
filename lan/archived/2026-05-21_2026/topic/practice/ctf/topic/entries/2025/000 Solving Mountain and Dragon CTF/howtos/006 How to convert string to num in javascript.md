# 1 Journal

* [x] 

From [^spawn-howto-01bbd2](006%20How%20to%20convert%20string%20to%20num%20in%20javascript.md#spawn-howto-01bbd2) in [3.9 Create a driver to search for death and infinity strings](006%20How%20to%20convert%20string%20to%20num%20in%20javascript.md#39-create-a-driver-to-search-for-death-and-infinity-strings)

2025-08-02 Wk 31 Sat - 06:11

From [w3schools reference](https://www.w3schools.com/js/js_type_conversion.asp),

We can use `Number("3.14")`. If it's not a valid number given like `Number("hi!")`, it should yield `NaN`.

Also this is an interesting error when dealing with `NaN`,

````ts
This condition will always return 'false'.ts(2845)

web_control.ts(176, 9): Did you mean 'Number.isNaN(m_addr16_to_access_or_nan)'?
````

We can't use ` ==` with them.
