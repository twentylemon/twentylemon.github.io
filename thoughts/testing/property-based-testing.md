Property Based Testing
----------------------

What a beaut, though.

In typical tests, one will create example inputs to a function and conjure outputs, asserting that the code gives those outputs when given the example inputs. This process is sane enough in a lot of cases, but suffers from the [test oracle](https://en.wikipedia.org/wiki/Test_oracle) problem. The goal of example based testing is to come up with enough examples to prove behaviours of the unit under test while providing enough coverage for edge cases and the like. Property based tests are also unit tests, but rather than provide input and output, a property is instead provided that the testing framework will try to disprove. Frameworks do that by generating biased random values, which one would use as the test input, or to construct the test input. Frameworks generate many tests, executing the code in the property to verify it's truthfulness.

Properties are non-trivial to come up with. You don't know the inputs to the functions any more, so it's typically not worthwhile to simply make assertions on the output. In order to get the output, you essentially have to run execute the unit under test, which defeats the purpose of proving it's correctness (_A_, therefore _A_ is not a great proof). Properties will prototypically take a few forms, one example of which is to combine related functions and assert on invariants. For example, consider a pair of functions `encode` and `decode`. A great property of these functions is that they must be inverse of each other. As a property, you can write that as (in python/[hypothesis](https://hypothesis.readthedocs.io/en/latest/)).

```python
@given(text())
def test_encode_decode_inverse(x):
  assert decode(encode(x)) == x
```

This property alone does not prove either `encode` or `decode` are _correct_, but it does provide a lot of evidence to show the two functions are inverse of each other, which every good encoding should be.

As another example, take simply addition. Addition has three great properties, it's commutative, associative, and has zero as the identity element. In code, that'd be:

```python
@given(integers(), integers())
def test_add_communative(a, b):
  assert add(a, b) == add(b, a)

@given(integers(), integers(), integers())
def test_add_associative(a, b, c):
  assert add(add(a, b), c) == add(a, add(b, c))

@given(integers())
def test_add_zero_identity(a):
  assert add(a, 0) == a
```

Fun fact, any function that satisfies all three of those property **must** be addition. Changing the identity element to `1` instead of `0` means the function must be multiplication.

Coming up with properties, in my experience, forces you to have a deeper understanding of the unit under test, incentivizes pure functions, and incentivizes simplicity. These are fantastic outcomes, even without the stronger evidence of correctness from the test suite.

## When They Fail

Property based tests using random inputs, so naively they'd seem to be less than useful when tests fail -- no one wants to try to trace where a random sized list of random strings ended up. However, this is not the case. Frameworks go out of their way to provide a minimally failing example case through a process called _shrinking_; taking a complex input and turning it into a simpler one. When a test fails, frameworks shrink the data over and over, checking the property each time to ensure it still fails. Eventually, it lands at an example that minimally fails which is extremely useful for debugging.

## Resources

* [How to Specify It](https://johanneslink.net/how-to-specify-it/) - guide for writing properties, specifically in java/[jqwik](https://jqwik.net/)
  * [original paper](how-to-specify-it.pdf) by John Hughes
