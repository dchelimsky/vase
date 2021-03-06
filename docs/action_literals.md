# Action Literals

This page describes the action literals and their parameters. For the
full scoop, you should definitely look at
src/com/cognitect/vase/literals.clj.

All the literals can be chained together in a single route, _except_
for #vase/respond and #vase/redirect.


## #vase/respond

Immediately send an HTTP response. This cannot be chained together
with any other literals.

| Param | Meaning |
| ----- | --------|
| :name | Name of the interceptor. Required. |
| :params | Symbols to bind from `:params`. |
| :headers | Any extra headers to merge with the defaults. |
| :edn-coerce | Symbols that should be parsed as EDN data before binding. |
| :body | An expression that produces the body of the response |
| :status  | The HTTP status code to return |

## #vase/redirect

Immediately send a 302 redirect response to the client.

| Param | Meaning |
|-------|---------|
| :name | Name of the interceptor. Required. |
| :params | Symbols to bind from `:params`. |
| :headers | Any extra headers to merge with the defaults. |
| :body | The body of the response |
| :status  | The HTTP status code to return |
| :url     | An expression that produces a URL. This will be the "Location" header sent to the client. |

## #vase/validate

Apply specs to the inputs.

| Param | Meaning |
|-------|---------|
| :name | Name of the interceptor. Required. |
| :params | Symbols to bind from `:params`. |
| :headers | Any extra headers to merge with the defaults. |
| :spec    | The spec to apply. Must be registered before this action executes.  |
| :request-params-path | Instead of locating parameters directly in the :request, use this path to navigate through nested maps.  |

## #vase/query

Run a datalog query and return the results as JSON

| Param | Meaning |
|-------|---------|
| :name | Name of the interceptor. Required. |
| :params | Symbols to bind from `:params`. |
| :headers | Any extra headers to merge with the defaults. |
| :edn-coerce | Symbols that should be parsed as EDN data before binding. |
| :query | A datalog query expression. Has access to symbols from `params`, in order, in the `:in` clause. |
| :constants | Additional values to be passed to the query, following all the parameters. |

## #vase/transact

Execute a Datomic transaction. Return the results (the tx-result) as JSON

| Param | Meaning |
|-------|---------|
| :name | Name of the interceptor. Required. |
| :properties | Whitelist of parameters to accept from the client |
| :headers | Any extra headers to merge with the defaults. |
| :db-op | Either :vase/assert-entity or :vase/retract-entity. Defaults to :vase/assert-entity. |

## #vase/intercept

Define an interceptor, given code in the descriptor file. The form
immediately after the literal is used directly as the interceptor
body.
