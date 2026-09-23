# 1 Objective

Create a credit store demo where the user is able to use shell commands to add new persons, delete persons, edit the credits a person has, and can have different levels of reports to group their transactions in which aggregate into prior reports.

They need to be able to have different currencies to manage, with their own registered persons: Credits, and (Arcade) Coins.

# 2 Goal

**User-facing**

* [ ] User can add, delete, and edit new persons

* [ ] User can add credit expenses and income

* [ ] User is able to start a new report and new expenses automatically go there

* [ ] User is able to conclude a report, and its aggregate is automatically produced

* [ ] User is able to create chains of reports whose aggregates are summed into parent report levels

* [ ] User can export a summary report for a given report level which only aggregates report levels higher than it

* [ ] Besides credits, user is also able to manage arcade coins in a similar fashion to credits with their own reports

* [ ] undo/redo functionality can be per transaction or per registered person per transaction

* [ ] Multiple users can interact with different historic versions of the data simultaneously in a safe threading fashion

**Developer-facing**

* [ ] Developer can add new managed tables in a similar fashion to credit or coin with minimal setup
