# 1 Journal

From [^spawn-issue-e23534](000%20npm%20audit%20reports%20security%20vulnerabilities%20for%20tutorial%20template%20project.md#spawn-issue-e23534) in [3.3 Follow with wweb static npm website tutorial](000%20npm%20audit%20reports%20security%20vulnerabilities%20for%20tutorial%20template%20project.md#33-follow-with-wweb-static-npm-website-tutorial)

2025-09-02 Wk 36 Tue - 10:07

Seems due to the versions being used.

````
23 vulnerabilities (1 moderate, 22 high)
````

````sh
npm audit fix --force
````

````
49 vulnerabilities (11 moderate, 28 high, 10 critical)
````

That just got worse!

````sh
npm audit fix
````

````
32 vulnerabilities (11 moderate, 17 high, 4 critical)
````

````sh
npm audit fix --force
````

````
21 vulnerabilities (1 moderate, 20 high)
````

This is strange how this is changing. It seems each force switches us to a different configuration with its own vulnerabilities. We definitely don't want the one with critical issues.
