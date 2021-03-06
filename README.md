# ipify.clj
[![CircleCI](https://circleci.com/gh/coldnew/ipify.clj.svg?style=svg)](https://circleci.com/gh/coldnew/ipify.clj)
[![Dependencies Status](https://jarkeeper.com/coldnew/ipify.clj/status.svg)](https://jarkeeper.com/coldnew/ipify.clj)
[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/coldnew/ipify-clj/master/LICENSE)

An **unofficial** client library for [https://www.ipify.org](https://www.ipify.org): A Simple IP Address API.

[![Clojars Project](http://clojars.org/coldnew/ipify/latest-version.svg)](http://clojars.org/coldnew/ipify)

## Dependencies

This library can worked on **both** Clojure/ClojureSript, you need following minimal version:

* Clojure 1.8.0 ↑
* ClojureScript 1.9.473 ↑

## Usage (Clojure)

In Clojure, we use synchronize method to retrive data from `https://api.ipify.org`. 

```clojure
(ns ipify-test.core
  (:require [coldnew.ipify :as ipify]))

(defn -main []
  ;; default return edn data
  (println "Public ip (default):" (ipify/get-public-ip))    ; => {:ip "98.207.254.136"}
  ;; You can specify the return type you want
  (println "Public ip (text):" (ipify/get-public-ip :text)) ; => "98.207.254.136"
  (println "Public ip (json):" (ipify/get-public-ip :json)) ; => {\"ip\": \"98.207.254.136\"}
  (println "Public ip (edn):"  (ipify/get-public-ip :edn))  ; => {:ip "98.207.254.136"}
  ) 
```

## Usage (ClojureScript on Browser/Node.js)

ClojureScript use **asynchronize** method instead, it will return [core.async](https://clojure.github.io/core.async) channel.
When a request has completed or failed it is put on that channel. You can take the response from that channel with the `<!` function **within** a `go` block.

```clojure
(ns ipify-test.core
  (:require-macros [cljs.core.async.macros :refer [go]])
  (:require [coldnew.ipify :as ipify]
            [cljs.core.async :as async]))

(go
  ;; return EDN format (default)
  (let [rsp (async/<! (ipify/get-public-ip))]
    (.log js/console "public ip (default): " rsp))  ; => {:ip "98.207.254.136"}

  ;; return EDN format (edn)
  (let [rsp (async/<! (ipify/get-public-ip :edn))]
    (.log js/console "public ip (edn): " rsp))      ; => {:ip "98.207.254.136"}
  
  ;; return TEXT format (text)
  (let [rsp (async/<! (ipify/get-public-ip :text))]
    (.log js/console "public ip (text): " rsp))     ; => "98.207.254.136"

  ;; return JSON-string format (json)
  (let [rsp (async/<! (ipify/get-public-ip :json))]
    (.log js/console "public ip (json): " rsp))     ; => {\"ip\": \"98.207.254.136\"}
  )
```

## Q & A

**Q**: Why I can't see return result on my browser ?

**A**: When use on browser, we use `jsonp` to fetch data. Please make sure your browser's `adblock` doesn't disable the `jsonp` call.

## License

Copyright © 2017 Yen-Chin, Lee <<coldnew.tw@gmail.com>>

Distributed under the [MIT License](http://opensource.org/licenses/MIT).
