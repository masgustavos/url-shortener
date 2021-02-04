# URL Shortener

This application was built while following the excelent video tutorials from [Tensor Programming](https://www.youtube.com/channel/UCYqCZOwHbnPwyjawKfE21wg) on [Go Design Patterns](https://www.youtube.com/playlist?list=PLJbE2Yu2zumAKLbWO3E2vKXDlQ8LT_R28), especifically when dealing with [Hexagonal Microservices](https://www.youtube.com/watch?v=rQnTtQZGpg8&list=PLJbE2Yu2zumAKLbWO3E2vKXDlQ8LT_R28&index=4).

Its goal is to receive an `url` as a parameter and return an 8 digit code, which can be used later on as a short URL.

To accomplish this goal, the application was developed in a way that it allows the use multiple repositories and serializers, via separation of logic and specific implementations, such as:

- Repositories:
  - MongoDB
  - Redis
- Serializers:
  - JSON
  - MessagePack

## Using the application

The applications expects some environment variables, which can be found in [docker-compose.yaml](docker-compose.yaml). One of great importance is `DATABASE_ENGINE` and it can assume the values `redis` or `mongo`.

Start the application with the command `docker-compose up`. The [Dockerfile.dev](Dockerfile.dev) executes [air](https://github.com/cosmtrek/air), so you can make changes in `*.go` files and enjoy live reloading.

- Use `curl` or `postman` to make a `POST` request and receive your code:

```sh
curl -XPOST http://localhost:8080/ -d '{"url": "https://github.com/masgustavos"}'
```

To simplify testing with MessagePack, there's a helper in [tool/msgpack.go](tool/msgpack.go). To execute it just run `docker-compose exec app go run tool/msgpack.go`

The response to both requests should look like this:

```json
{"code":"e7iYk_YMg","url":"https://github.com/masgustavos","created_at":1612468085}
```

- With the code in hand, open your browser and use it to be redirected to the previously provided URL, e.g.: `http://localhost:8080/e7iYk_YMg`
