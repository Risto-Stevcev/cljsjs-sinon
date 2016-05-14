# cljsjs/sinon

[](dependency)
```clojure
[cljsjs/sinon "1.17.3-0"] ;; latest release
```
[](/dependency)

After adding the above dependency to your project you can require the packaged library like so:

```clojure
(ns application.core
  (:require cljsjs.sinon))
```

## Examples

```clojure
(ns test-sinon.core
  (:require [cljsjs.sinon]
            [ajax.core :refer [GET]]))

(enable-console-print!)


;; Create a spy
(def myobj (clj->js {"hello" (fn [name] (str "hello, " name "!"))}))
(def spy (js/sinon.spy myobj "hello"))

(assert (false? (.-calledOnce spy)))
(println (myobj.hello "mister")) ; hello, mister!
(assert (true? (.-calledOnce spy)))
(assert (true? (.calledWith spy "mister")))


;; Create a fake server
(def server (js/sinon.fakeServer.create))
(def mock-response [{"id" 12 "comment" "Hey there"}])

(.respondWith server "GET" "/some/article/comments.json"
  (clj->js [200 {"Content-Type" "application/json"} (-> mock-response clj->js js/JSON.stringify)]))

(def callback (js/sinon.spy))
(GET "/some/article/comments.json" {:handler callback})
(.respond server)
(js/sinon.assert.calledWith callback mock-response)
```
