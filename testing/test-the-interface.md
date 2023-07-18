Test the Interface, not the Implementation
------------------------------------------

---

Don't `spy` on the unit under test. Tests are meant to prove the existance of a given behaviour of a unit of logic (or disprove non-existance), changing the unit while testing it is counter productive.

---

`private` implementation details need to stay `private`. If you feel inclined to make something _visible for testing_, consider refactoring to simplify the interface you are testing. Consider adding more components in order to have smaller chunks to test.

---

Tests are the first consumer of the code. Writing the interface first can help simplify your testing, but can also help simplify usage of the unit in the future.

---

Tests exhibit the behaviour of a unit, and thus make great examples for those working in the same code base. Aim to exhibit all behaviours of a unit, don't exclusively aim for code coverage.

---
