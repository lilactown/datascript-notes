# Notes on Transacting

## Transacting nested maps

Transacting a bunch of nested data _Just Works™️_. If you specify your schema well,
DataScript will do the work of denormalizing it into the database for you.


```clojure
(def schema
  {:user/id {:db/unique :db.unique/identity}
   :user/friends {:db/cardinality :db.cardinality/many
                  :db/valueType :db.type/ref}})

(def db (ds/empty-db schema))

(ds/db-with
 db
 [{:user/id 1
   :user/name "Will"
   :user/friends [{:user/id 2
                   :user/name "Beth"
                   :user/friends [{:user/id 3
                                   :usern/name "Uma"}]}]}])
```

Results:
```clojure
#datascript/DB {:schema {:user/id {:db/unique :db.unique/identity},
                         :user/friends {:db/cardinality :db.cardinality/many, :db/valueType :db.type/ref}},
                :datoms [[1 :user/friends 2 536870913]
                         [1 :user/id 1 536870913]
                         [1 :user/name "Will" 536870913]
                         [2 :user/friends 3 536870913]
                         [2 :user/id 2 536870913]
                         [2 :user/name "Beth" 536870913]
                         [3 :user/id 3 536870913]
                         [3 :usern/name "Uma" 536870913]]}
```






