The backend system behide the simple front page of goole is gigantic and powerful like the monster of Gozilla. Such extremely sophisticated system is not come into being at first hand.
Like all creatures of our world, they are all enovle from a very simple form as a single cell. Since we want to design a powerful system that can coordinate thounsands of machines into
one to complete any complicated task, we need to know how to create and mantain a single server on a single machine, this is just like we will push the evolution of an advanced creature
from a single cell.

Let's quickly build a simple hello world server as following, create a forlder with name microservice-distribute-system and create a file with name main.go in it, 
then have the followng code:

```go
package main

import (
	"fmt"
	"log"
	"net/http"
)

func main() {
	port := 8080

	http.HandleFunc("/hello", helloHandler)

	log.Printf("Server starting on %v\n", port)
	log.Fatal(http.ListenAndServe(fmt.Sprintf(":%v", port), nil))
}

func helloHandler(w http.ResponseWriter, r *http.Request) {
	//return a string directly for the request
	fmt.Fprintf(w, "Hello here for you")
}
```

Then run the code with command:
"go run main.go"
When the server starts, it will print out "Server starting on 8080", then we can open the browser and open the page with link http://localhost:8080/hell, and you will see a string
"Hello here for you" showing on the brank page.

Now we have a server starts up, then we need the server to receive request and handle the request by returning something, the most common way of doing this is by receiving and sending
out data in json format, and we change above code to following:

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"encoding/json"
)

func main() {
	port := 8080

	http.HandleFunc("/hello", helloHandler)

	log.Printf("Server starting on %v\n", port)
	log.Fatal(http.ListenAndServe(fmt.Sprintf(":%v", port), nil))
}

type HelloResponse struct {
	Message string
}

func helloHandler(w http.ResponseWriter, r *http.Request) {
	//return a string directly for the request
	//fmt.Fprintf(w, "Hello here for you")
	response := HelloResponse {
		Message: "hello",
	}
	//convert struct to json string
	data, err := json.Marshal(response)
	if err != nil {
		panic("error for converting json")
	}
	fmt.Fprintf(w, string(data))
}
```
Then run the code and refresh the page again and we will see following output:

{"Message":"hello"}

That is the convertion just translated the object with its field and value into strings. How about we want to change the name of field from "message" to "welcome", we can change the
definiton of the HelloResponse as following:

```go
type HelloResponse struct {
	Message string `json:"welcome"`
}
```
Its quit handful when we can change the convertion of struct to json in this way. Actually we can make the code simplier by combine the convertion of json and sending out the response
data into one step as following:

```go
func helloHandler(w http.ResponseWriter, r *http.Request) {
	//return a string directly for the request
	response := HelloResponse {
		Message: "hello",
	}

	//using Encoder to convert and send
	encoder := json.NewEncoder(w)
	encoder.Encode(response)
}
```
As you can see the code above is much handy then before.


