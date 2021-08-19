# Train Sim Stripe

A work-in-progress application to simulate purchasing train tickets. 
This project is a support project of Train Sim, which provides a fake stripe service that allow other services to check the price of an itinerary and make payments.

Please do not hesitate to contact the TAs (especially Jason Lefever or Hongzhou Fang) with any questions.

## Getting Started
The following tools are required to build and run this project: Docker, Docker Compose, Maven, Java 8, and npm.

From the root of this directory run:

```
mvn clean install
docker-compose up
```

The service will start at http://localhost:8003/. You can also config the port from docker-compose.yml.

You can also directly run App.java from your IDE. The service will start at http://localhost:80/.

## API

### /api/stripe/price
The API which provides price checking of a given itinerary.
It only receive http post request with the following body:

```
{
    routeIDs: [...]
} 
```

`routeIDs` is a list of leg routeIDs in one itinerary. If the request body does not have field `routeIDs`, the server will return `400`. 
Otherwise, the service will just return the price which value is `5 * # of routeIDs` in following format:
```
{
    price: 5
}
```

### /api/stripe/payment
The API which fakes a payment interface.
It only receive http post request with the following body:

```
{
    paymentAmount: 5,
    creditCardInfo: {
        cardNumber: "1111222233334444",
        cvv: "123"
    }
} 
```
`paymentAmount` is the amount of dollar that will be charged from the credit card. 
The server will validate `cardNumber` field in `creditCardInfo` by checking if it is a 16-digit string. 
It will also validate `cvv` field by checking if it is a 3-digit string.
If any field is missing, the server will return `400`. If the validation fails, server will return:
```
{
    code: 2,
    message: "Credit card validation failed"
}
```
Otherwise, it will return:
```
{
    code: 0,
    message: "Payment succeed"
}
```