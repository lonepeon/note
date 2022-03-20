# Avoid named labels

At school I’ve been told how harmful this feature is and that we shouldn’t use it.
Then, at work, I had to maintain old applications written in Java / JavaScript where developers were used to this mainly because it was the closest thing to GOTO they were used to have in C/C++. 
It has been a nightmare for me to follow the nested loops with several labels, confirming what I’ve been told at school.

Since then I haven’t seen this feature used in any program I had to maintain and was quite happy with it to be honest. 
Most young Java/JavaScript developers at this time moved to the "extract to another function" instead, to make things more manageable. 
It forced us to keep nesting small in each context and better express our intent through naming. 
In the case of infinite for loop and select it's not exactly the same problem but the solution can be the same:

```go
// Avoid
func main() {
    timer := time.NewTimer(time.Duration(15) * time.Second)
RETRY: 
    for {
        time.Sleep(time.Duration(1) * time.Second)
        
        select {
            case serverErr := <-serverErrorChan:
                if serverErr.Error != nil {
                    t.Fatalf("application failed: %+v", serverErr)
                }
            case <-timer.C:
                t.Fatalf("e2e test api server is unhealthy")
            default:
                resp, err := http.Get("http://localhost:9000/ping")
                if err == nil && resp != nil && resp.StatusCode == 200 {
                    break RETRY
                }
        }
    }
}

// Prefer
func main() {
    timer := time.NewTimer(time.Duration(15) * time.Second)
    for {
        time.Sleep(time.Duration(1) * time.Second)

        isHealthy, err := checkServerIsHealthy(timer, serverErrorChan)
        if err != nil {
            t.Fatalf(err)
        }

        if isHealthy {
            break
        }
    }
}

func checkServerIsHealthy(timer *time.Timer, serverErrorChan chan web.APIServerError) (bool, error) {
    select {
    case serverErr := <-serverErrorChan:
         if serverErr.Error != nil {
             return false, fmt.Errorf("application failed: %+v", serverErr)
        }
    case <-timer.C:
        return false, errors.New("e2e test api server is unhealthy")
    default:
        resp, err := http.Get("http://localhost:9000/ping")
        if err == nil && resp != nil && resp.StatusCode == 200 {
            return true, nil
        }
        return false, nil
    }
}
```

I have no doubt this feature can be useful for some context other than Web where Go is used but my main issue is that it comes with a cost on readability/maintainability.

When used only basically we can all understand what it does but, if we allow it to enter our toolset, we will need to define the rules so it doesn't become a hell.

I would argue the "extract to a function" make things clearer as it forces us to clarify the intent so I would rather not have any new comer think about this feature and learn how to use properly.

All features are not here to be used by all developers like this break with label, goto or the dot import, exactly the same way as in JavaScript where this feature is present and almost never used.

Finally I think we should always try to lighten our cognitive load regarding the way we design our code, the patterns we use etc... Our goal is to add value by writing code that is explicit and understandable by everyone. 
We already have a lot to keep in mind (how to structure our services, how to trace, how to handle errors, how to test, how to document our APIs,...), so let's keep it simple by not adding an alternative to a pattern we already know.
