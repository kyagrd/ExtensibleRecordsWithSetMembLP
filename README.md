# ExtensibleRecordsWithSetMembLP

Okay, this is a title for a new paper:

  Executable Relational Specification of Extensible Records with Set-Membership in Logic Programming

or, in short,

  ER-spec of Extensible Records with Set-Membership in LP

Now I have a pretty concrete idea and at least I think I can develop an ER-spec
of Extensible Records with Prolog (of course relying on some non-logical hacks).

The Prolog code examples implementing set unification from set membersihp are posted online
  * http://ideone.com/uNHNYs (a naive one)
  * http://ideone.com/SyDfCy (a better one)

which is based on
"Membership-Constraints and Complexity in Logic Programming with Sets", Frieder Stolzenburg (1996).
The paper is available online at either
http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.54.8356
or
http://link.springer.com/chapter/10.1007%2F978-94-009-0349-4_15
(if you paid springer).

Although it is possible to implement set unification as a library like this, it is not ideal.
What I really want is a LP system that supports set unification (where order and duplication are irrelevant) as a primitive operation as well as structural unification (where order and dupliation matters), and furthermore, set-values and ordinary structural values in traditional LP should be able to be mix in together. That is, structural compound term can contain sets as its argument as well as other structural values. And of course, set should be able to contain both structural values and set values. So, there should be a universal unification operator that does set unification when arguments are sets and does structural unification when arguments are structural.

Set unification is essentional but may be not enough to handle type inference including records.
Let's use a notation like this, uniyfing two records types.
(Yes, I know that Prolog uses curely braces for tuples, but
I can't think of a better syntax that is backward compatible with Prolog).

    {x:int, y:T1 | R1} = {y:T2, z:int | R2}

We expect the result to be `R1 = {z:int|R}`, `R2 = {x:int|R}`,
`T1 = int`, and `T2 = int`, which can be computed by set unification.
However, consider

    {x:int | R1} = {x:bool | R2}

If we simply consider `x:int` and `x:bool` as structural compound terms,
which is the case in Prolog, the set unification results in `R1 = {x:bool|R}` and `R2 = {x:int|R}`
unifiying lhs and rhs to `{x:int, x:bool | R}`. This is not what we want for structural typing of records.
What we want is map unification where the unification should fail if different values (`int` and `bool`
in this example) are mapped from the same key (`x` in this example).
I was able to implemnt map unification in following a similar code structure, also posted online
 * http://ideone.com/GAcIVa

which I think is our contribution (maybe someone have been using a similar thing in other context, but for extensibe records this would be a unique appraoch so far, I think).

Now that we have implemented both unifications for sets and maps, which could be open ended,
how much do we need to support as primitives and as a library.
I definitely feel that set unification is worthwhile to be supported as a primitive operation, but what about map unification?
Can we efficiently and declaratively implement map unification in terms of set unification and ordinary unfication, or, is it also worthwhil to support as a primitive operation as well in logic programming?

There is a related work in a more practical setting.
Clojure's `core.logic` implements (partial) map unification, but not as described above
rather like this http://clojure-log.n01se.net/date/2013-02-09.html
which I think is a wrong design (it is not transitive, cannot really call it unification).
It should be designed like open-ended set unification.
