# How to run

This guide is following the following resources:

- https://www.youtube.com/watch?v=ICUSfTorBnI
- https://www.youtube.com/watch?v=Rf1VewhIrqM
- https://www.cargo-lambda.info/guide/getting-started.html

And modified to run on Docker.

## Pull the cargo-lambda image

```sh
docker pull ghcr.io/cargo-lambda/cargo-lambda
```

## Run the cargo-lambda container

```sh
docker run --rm -it \
-v /path/to/your/git/workspace:/app \
-w /app \
--network=host \
--name=cargo-lambda \
ghcr.io/cargo-lambda/cargo-lambda sh
```

You can interact with the container from other terminal with the following command:

```sh
docker exec -it cargo-lambda sh
``

## Create a new project with cargo-lambda

From your interactive docker container shell, run the following command:

```sh
cargo lambda new my-first-http-lambda # Answer `Yes` to the question `Is this function an HTTP function?`
cd my-first-http-lambda
```

Now you can open `/path/to/your/git/workspace/my-first-http-lambda` with your favorite editor.

## Build the project

```sh
cargo lambda build --release
```

```sh
AWS_ACCESS_KEY_ID=<YOUR_USERS_AWS_ACCESS_KEY_ID> AWS_SECRET_ACCESS_KEY=<YOUR_USERS_AWS_SECRET_ACCESS_KEY> cargo lambda deploy --enable-function-url --iam-role <YOUR_FULL_IAM_ROLE_ARN_WITH_AWSLambdaBasicExecutionRole>
# 🔍 function arn: <ARN_OF_YOUR_DEPLOYED_FUNCTION> 🔗 function url: <URL_OF_YOUR_DEPLOYED_FUNCTION>
```

Now, you can access the function url from your browser.
If you add `?name=JohnDoe` to the end of the url, you will see `Hello JohnDoe, this is an AWS Lambda HTTP request`.

## Run the cargo-lambda watch command

From your interactive docker container shell used above, run the following command:

```sh
cargo lambda watch
```

## Run the cargo lambda invoke command

Inside the container, run the following command:

```sh
cargo lambda invoke --data-example apigw-request
# {"statusCode":200,"headers":{},"multiValueHeaders":{"content-type":["text/html"]},"body":"Hello me, this is an AWS Lambda HTTP request","isBase64Encoded":false}
```

Or,

```sh
curl http://localhost:9000?name=JohnDoe
# Hello JohnDoe, this is an AWS Lambda HTTP request
```

You can use this for development.
