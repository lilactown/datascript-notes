# Notes on Querying

## Getting the right number of results

Sometimes a query should return many results, sometimes one result.
Sometimes you only care about whether a result exists at all!

The `:find` spec allows you to control how many results a query returns.

Given the following database with a few entities:

```clojure
(def db
 (ds/db-with
   (ds/empty-db)
    [{:foo "bar"}
     {:foo "baz"}
     {:foo "asdf"}
     {:foo "jkl"}]))
```

### All results

```clojure
(ds/q
 '[:find ?e ?foo
   :where
   [?e :foo ?foo]]
  db)
;; => #{[1 "bar"] [2 "baz"] [3 "asdf"] [4 "jkl"]}
```

### First result

```clojure
(ds/q
 '[:find [?e ?foo]
   :where
   [?e :foo ?foo]]
 db)
;; => [1 "bar"]
```

### A single scalar result

If we our result is a single value, e.g. `?foo`, then we can use the `.`
to get a single scalar value out.

```clojure
(ds/q
 '[:find ?foo .
   :where
   [?e :foo ?foo]]
 db)
;; => "bar"
```
