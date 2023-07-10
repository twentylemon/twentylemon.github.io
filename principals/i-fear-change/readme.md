I Fear Change; Life is Pain
---------------------------

---

Things can't break if they do not change.  
Mostly. Changing constantly is the hardware beneath.

---

I know things better if they have not changed.

---

Consider the expression,

```lisp
(= (map f (reverse list)) (reverse (map f list)))
```

When is the expression true? As you think about it, you'll realize that, of course, the two operands are always equal, thus the expression is always true. However, a hidden assumption was made -- what is `f`? In the first operand, `f` is applied to the reverse list. In the second, `f` is applied to the list directly. These operands **may** not be equal if `f` is impure. That condition requires substantially more mental effort to understand this seemingly trivial expression.

---
