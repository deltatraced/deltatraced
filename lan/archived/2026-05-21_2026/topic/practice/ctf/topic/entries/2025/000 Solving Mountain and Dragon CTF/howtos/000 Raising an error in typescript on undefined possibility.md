# 1 Journal

* [x] 

2025-07-30 Wk 31 Wed - 05:05

Trying to go from javascript to typescript I'm encountering some unhandled undefined errors, like

````typescript
'g_mq' is possibly 'null'.ts(18047)
````

We do not want to alter this functionality beyond the necessary, so let's throw an error on those sorts of possibilities.

As in this [stackoverflow answer](https://stackoverflow.com/a/38633821/6944447),

We can throw a `TypeError`:

````typescript
throw new TypeError('Error message');
````
