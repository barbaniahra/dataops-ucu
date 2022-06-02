## Sample application 
https://developer.lightbend.com/guides/akka-http-quickstart-scala/

## Building the image
- `docker build -t sample_app .`

## Running the image:
- `docker run -p 8080:8080 sample_app`

## Testing app works
- Creating 3 users:
```
curl -H "Content-type: application/json" -X POST -d '{"name": "MrX", "age": 31, "countryOfResidence": "Canada"}' http://localhost:8080/users
curl -H "Content-type: application/json" -X POST -d '{"name": "Anonymous", "age": 55, "countryOfResidence": "Iceland"}' http://localhost:8080/users
curl -H "Content-type: application/json" -X POST -d '{"name": "Bill", "age": 67, "countryOfResidence": "USA"}' http://localhost:8080/users
```

- List users
```
curl http://localhost:8080/users
```

- Expected output (prettified):
```
{
   "users" : [
      {
         "age" : 31,
         "countryOfResidence" : "Canada",
         "name" : "MrX"
      },
      {
         "age" : 55,
         "countryOfResidence" : "Iceland",
         "name" : "Anonymous"
      },
      {
         "age" : 67,
         "countryOfResidence" : "USA",
         "name" : "Bill"
      }
   ]
}
```