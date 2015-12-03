# Specification of Extensible Records with Set-Membership in Logic Programming

okay this is a title for a new paper now I have a pretty concrete idea and at least I can implement with Prolog (of course with some non-logical built-in hacks)

The Prolog code for set membersihp is posted online
  * http://ideone.com/uNHNYs (a naive one)
  * http://ideone.com/SyDfCy (a better one)

Although it is possible to implement set unification as a library in this way, this is not ideal.

What I really need is a LP system that supports set unification (where order and duplication are irrelevant) as a primitive operation as well as structural unification (where order and dupliation matters), and furthermore, set-values and ordinary structural values in traditional LP should be able to be mix in together. That is, structural compound term can contain sets as its argument as well as other structural values. And of course, set should be able to contain both structural values and set values. So, there should be a universal unification operator that does set unification when arguments are sets and does structural unification when arguments are structural.
