`final` is Needless Noise, Change my Mind
-----------------------------------------

---

> I like the idea of explicitly declaring that a variable is not intended to be assignable (less so for function parameters) - also thinking of languages like Kotlin force you to express this intent (`var` vs `val`).

The main argument I tend to see is that is somehow "safer" to make every local variable and every parameter `final`. But, `final` is an extraordinarily weak modifier in java, I am free to change the value on a whim (think of a list). Languages like kotlin/scala that have `val` also prefer immutable values and immutable composites. I am one billion percent on board with immutable composites, but I'd rather avoid putting up the pseudo guardrails java likes to impose. `final` local variables and parameters leads to none of the guarantees languages like kotlin or scala provide, while lying to you and saying it does.

PLUS it's verbose.

A fun blog post from many years ago: http://steve-yegge.blogspot.com/2010/07/wikileaks-to-leak-5000-open-source-java.html

> Thanks for sharing the blog post! It's true that final doesn't provide full immutability in the case of non-primitive variables (in the same way that `val` doesn't) but its part of the immutability puzzle (e.g., final ImmutableList myList, where ImmutableList provides immutability of the list, and final provides immutability of the list reference).

This falls into the points I raised below; immutability of a reference, while typically preferable, I don't believe should be enforced. I don't tend to see a huge amount of value arising from _enforcing_ all local variables and parameters are not able to be re-assigned. (And yes, I lived in the haskell ivory tower. I am in favour of monadic IO :gasp:)
I interpret this point as more of an argument to move toward languages with language level or idiomatic support for true immutability (like in scala/kotlin [scala moreso, but OO languages will always have stateful composites], clojure, etc). But for java, where support is just third parties, the idiom is mutability.

As mentioned, I am 1390284759823745% on board with immutable composites, they are, in my experience, the valuable part of immutability. There are, however, plenty of cases where mutability is drastically simpler (A* search pops into mind, for example).

---

I tend to disagree that you should not reassign arguments. A trivial example,

```java
void func(String input) {
  input = input.toLowerCase(); // or `input = validate(input)` etc
  /* ... */
}
```

> Reassigning variables, especially method arguments, make debugging unnecessarily hard. afaik, its generally not considered as a good practice. Even without explicit `final` keywords, I would prefer to write code which assumes all variables are essentially final.

I'd tend to disagree, re reassigning arugments. Keeping the local namespace less cluttered is sane, and there's no reason to keep two values around that are _almost_ identical, but one of which is objectively _wrong_ to use. In my example above, defining `String inputLower = input.toLowerCase()` provides no benefit, and allows you to misuse `input` instead of `inputLower` -- _that_ makes debugging unnecessarily hard.

As for good practice, I simply don't buy it. Plenty of languages adopt this as a standard idiom when they don't have default function arguments. For example in javascript, `in = in || someDefault`. For _best practices_ in general, I tend to argue against assertions such as "x is bad, don't do it." Rather, I prefer advice that knows that it makes sense to not follow the advice when the situation calls. [A nice write up](https://www.scattered-thoughts.net/writing/on-bad-advice/#what-lasts) on this specifically; the whole piece is worth a read as well.

And, let's be clear, I'm not advocating we start writing 1960s style micro-optimized C code, I tend to avoid reassignments (I am one of those functional programming nerds). Instead, I argue that, sometimes, it is cleaner and easier to understand when you do write things in an imperitive style, reassignments et al.

---
