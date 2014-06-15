---
layout: page
title: An Easier Racket Reference
tagline: "(contract violation)"
category: Programming_Language
tags: [racket, guide]
permalink: /racket-easy-ref/
last_updated: Sun, 15 Jun 2014 20:17:31 +0700
---
{% include JB/setup %}

This is a version of the Racket reference with *better* understanding and
useful examples.

## 3. Syntactic Forms ##

### Local Binding, `let`, `let*`, `letrec`, ... ###

Original link: http://docs.racket-lang.org/reference/let.html

* With the simplest `let` form:

  ```racket
  (let ([id val-expr] ...)
    body ...+)
  ```

  This:

  - evaluates all `val-expr`s,
  - create a new `location` for each `id`,
  - places the evaluated values into those `location`s,
  - binds `id`s to those values lexically,
  - then evaluates `body` and returns the value of the last expression.

  Something to keep in mind:

  - `id`s must be distinct.
  - `let` form is lexically scoped.
  - All bindings are **not** sequential.

  E.g.

  ```racket
  (let ([x 5]) x)     ;; => 5

  (let ([x 5])
    (let ([x 2]
          [y x])
      (list y x)))    ;; => '(5 2)

  ;; Not working since bindings are not sequential
  (let ([x 5]
        [y x])
    (list y x))
  ;; =>
  ; x: undefined;
  ;  cannot reference undefined identifier
  ; [,bt for context]
  ```

* `let proc-id`:

  ```racket
  (let proc-id ([id init-expr] ...)
    body ...+)
  ```

  This form binds `proc-id` to a function `(λ (id init-expr ...) body ...+)`
  with `id`s as its arguments that take initial values `init-expr`s.  The
  binding process works as in `let`.

  E.g.

  ```racket
  (let fact ([n 10])
    (if (zero? n)
        1
        (* n (fact (sub1 n)))))
  ;; => 3628800
  ```
