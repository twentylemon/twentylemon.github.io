You're Probably Mocking too Much
--------------------------------

The norm for testing nowadays is to use some form of mock to isolate the unit under test. Fowler's [Test Double](https://martinfowler.com/bliki/TestDouble.html) post probably sparked the industry on a path to where it is now, along with a few of his other writings on the subject. I used to be an avid follower of this idea, but I am of the opinion now that test doubles, of any form, are extremely overused.

The majority of tests I see that use mocks look something like the following,

```java
@Test
public void getThings_onlyUseCase_returnsThings() {
  Collection<Thing> things = List.of(new Thing());
  String serialized = "things";
  when(http.get("...")).thenReturn(serialized);
  when(parser.parse(serialized)).thenReturn(things);

  Collection<Thing> actual = classUnderTest.getThings();
  assertThat(actual).isEqualTo(things);
  verify(http).get("...");
  verify(parser).parse(serialized);
  verifyNoMoreInteractions(http, parser);
}
```

Given this test, we can extract how the source code is written.

```java
public Collection<Thing> getThings() {
  return parser.parse(http.get("..."));
}
```

It is my opinion now that such tests do not actual _test_ anything meaningful. No behaviour of the unit under test is proven, or proven to not exist. Rather, we essentially prove the source code is written the way it was written. As soon as we change any detail in `getThings`' implementation (say by inlining `parser.parse`), the tests fail. There are hints to this in the test itself, why was `things` initialized in that way; why is the serialization of it `"things"`? The test doesn't meaningfully change when those values are changed, the test is independent of the data used in it.

Removing the mocks would eliminate the issue. Run the unit the same as production would and assert the output of the unit, this verifies the unit's actual behaviour. But, this introduces another issue. How can we test this unit _without_ mocks? The parser mock is trivially removable, but the http client an http request. This unit is impure, in the functional programming sense (it relies on some IO).

### Making it Pure

Every impure function can be made pure by promoting any _implicit_ inputs the function has. Let's take an example,

```java
public void Thing getThingById(String id) {
  String thing = http.get("?id=" + id);
  return parser.parseThing(thing);
}
```

Functions have _explicit_ inputs, these are just the parameters; `id` in this example. They also have _implicit_ inputs; `String thing` here. The behaviour of `getThingById` depends on these two inputs, and then produces some output, so it's pure if we consider both types of inputs. This function can be made into an actual pure function by promoting the implicit input to an explicit one.

```java
public void Thing getThingById(String id, String thing) {
  return parser.parseThing(thing);
}
```

You'll notice now that `id` is unused, so we can remove it.

```java
public void Thing getThingById(String thing) {
  return parser.parseThing(thing);
}
```

You'll also notice now that this whole function is just `parser.parseThing`, which is indeed a pure function. But, we also don't need this function at all any more as it is just a synonym of another function.

But ok. We still need to do the http call. The _output_ of the http call is now the _input_ to this function, so the caller of this function needs to now be the one to make that http call. We may also want the calling function to be pure, the same mechanism can be re-run on it, turning implicit inputs into explicit ones, pushing impurities up the call stack until eventually we land in `main`. (It does not have to be literally `main`, but whatever you consider to be the entry point, like a REST endpoint, AWS Lambda handler, etc.)

Through this, we turned a whole chain of impure functions into a chain of pure functions instead, fun stuff. The tests we write for each of those pure functions can be descriptive, free of mocks and full of proving behaviours. [Property based tests](property-based-testing.md) work phenominally for pure functions, by the by.

We also pushed impurities to `main`. `main` is the ultimate impure function, it is _always_ impure, as some form of IO had to occur for it to even run in the first place. There's no better place to cram all your impurities! That is not to say, make your `main` hundreds of lines long, you should still organize with some sanity (not _every_ function has to be pure, but your business logic probably should be). But, that is to say, do your IO in `main`; http requests, caching logic, that sort of thing. Do some IO, delegate to your newly pure business logic for a data transformation, repeat. If `main` gets too large, it may be your application, API, etc, may be getting too complex, and you may want to consider breaking it apart into more pieces.

### etc
* [47deg - Pitfalls of Mocking in tests](https://www.47deg.com/blog/mocking-and-how-to-avoid-it/)
