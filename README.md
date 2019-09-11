Vela
========

Simple web framework to build restful app in Racket.

Features
------------
- Write handlers use Function Based Handler or Class Based Handler.
- Friendly way to define url routers.
- Json response maker.
- Entirely on the racket webserver lib.


Installation
------------

`raco pkg install vela`

Quick code
-----------

```racket
#lang racket
(require vela)


(define hello-handler
  (class handler%

    (define/public (get [id null])
      (displayln id)
      (jsonify (hash 'code 200 'msg "handle get" )))

    (define/public (post)
      (jsonify (hash 'code 200 'msg "handle post" )))

    (define/public (put id)
      (jsonify (hash 'code 200 'msg "handle put" )))

    (define/public (delete id)
      (jsonify (hash 'code 200 'msg "handle delete" )))

    (super-new)))


(define index-handler
  (lambda (req)
    (jsonify (hash 'code 200 'msg "hello api" ))))


(define api-v1 (url-group "/api/v1")) ;define a url group

(define routers
  (urls

    (url "/" index-handler "handler with function")

    (api-v1
      (url "/hellos" hello-handler "hello-list/post")
      (url "/hello/:id" hello-handler "hello-put/delete/get"))))


(app-run
  routers
  #:port 8000
  #:log-file "access.log" ;your log file
  #:static-path (build-path (current-directory) "static") ;your static files dir
  #:static-url "static") ;your static url suffix

```


examples
----------
Very simple apps build with Vela in the [demos folder](https://github.com/nuty/vela/tree/master/demos).


License
-------
Licensed under the MIT License.
