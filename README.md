# redis-lock

Simplified distributed locking implementation using [Redis](http://redis.io/topics/distlock).
For more information, please see examples.

## Examples

```go
import (
  "fmt"
  "time"

  "github.com/bsm/redis-lock"
  "gopkg.in/redis.v5"
)

func main() {
	// Connect to Redis
	client := redis.NewClient(&redis.Options{
		Network:	"tcp",
		Addr:		"127.0.0.1:6379",
	})
	defer client.Close()

	// Obtain a new lock with default settings
	lock, err := lock.Obtain(client, "lock.foo", nil)
	if err != nil {
		fmt.Printf("ERROR: %s\n", err.Error())
		return
	} else if lock == nil {
		fmt.Println("ERROR: could not obtain lock")
		return
	}

	// Don't forget to unlock in the end
	defer lock.Unlock()

	// Run something
	fmt.Println("I have a lock!")
	time.Sleep(200 * time.Millisecond)

	// Renew your lock
	ok, err := lock.Lock()
	if err != nil {
		fmt.Printf("ERROR: %s\n", err.Error())
		return
	} else if !ok {
		fmt.Println("ERROR: could not renew lock")
		return
	}
	fmt.Println("I have renewed my lock!")

}
```

## Documentation

Full documentation is available on [GoDoc](http://godoc.org/github.com/bsm/redis-lock)

## Testing

Simply run:

    make


