# Bind operator proposal

This proposal adds new :: operator for argument binding 
for marlang programming language.

Examples:
```ts
do_math: (arg1: 128, arg2: 128, arg3: 128): 128 = arg1 + arg2 - arg3
bound: do_math::(1234, 432)
printf(bound(123)) // 1543
```
```ts
print_error: printf::("[ERROR]")
// some code later
print_error("Unacceptable value", value)
// prints: [ERROR] Unacceptable value <value>
```

Semantics:

```
BindExpression:
    LeftHandSideFunction<infer Arguments> :: CallExpression<Partial<Arguments>>
```

RuntimeEvaluation:
 - let F be the LeftHandSideFunction
 - let B be the new Function with remaining parameters that weren't supplied in call 
 - let Args be parameters, that were supplied in function call
 - When B is evaluated perform next steps:
     - let Result be result of Call(F, <<...Args, ...arguments>>)
     - return Result
 - return B
