# Make code grepable

We need to make sure we are able to easily find tokens in the code. 
It's especially important when the applications experience issues in production.

What does this mean:

1. Avoid using dynamic function definition using string interpolation
2. Avoid doing dynamic function call invocation based on string interpolation
3. Avoid using name that are not clear `x`, `a`, etc... but use name we can search for.

In Go I found another patterm that happens a lot when dealing with error wrapping:

```go
var ErrExposedBusinessFailure = errors.New("something bad happened")

func MyFunction() error {
    if err := doSomething(); err != nil {
        return fmt.Errorf("%w: %v", ErrExposedBusinessFailure, err)
    }
    
    if err := doSomethingElse(); err != nil {
        return fmt.Errorf("%w: %v", ErrExposedBusinessFailure, err)
    }
}
```

When the error is reported it can look like this:

```text
something bad happened: an explanation on what's wrong
something bad happened: another explanation on what wrong
```

If we search for`something bad happened` we will find the error definition. 
We will then have to find all occurrences of `ErrExposedBusinessFailure`  and guess which one occurred.

The fix is to add a bit more context to the error we send back so we can easily discriminate which line failed. 
The error is a bit more verbose but it builds a complete business stack trace we can understand and search for.

```go
var ErrExposedBusinessFailure = errors.New("something bad happened")

func MyFunction() error {
    if err := doSomething(); err != nil {
        return fmt.Errorf("can't do something: %w: %v", ErrExposedBusinessFailure, err)
    }
    
    if err := doSomethingElse(); err != nil {
        return fmt.Errorf("can't do something else: %w: %v", ErrExposedBusinessFailure, err)
    }
}
```

Now, the errors will reported as follow. 
We are now able to pinpoint the exact function call we were looking for.

```text
do something: something bad happened: an explanation on what's wrong
do something else: something bad happened: another explanation on what wrong
```
