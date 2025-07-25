# Skinet Project Repository

Welcome to the brand new version of the SkiNet app created for the Udemy training course available [here](https://www.udemy.com/course/learn-to-build-an-e-commerce-app-with-net-core-and-angular).

This app is built using .Net 9 and Angular 20 

# Running the project

You can see a live demo of this project [here](https://skinet-course.azurewebsites.net/).

You can also run this app locally.  To run this project locally you will need to have installed:

1. Docker
2. .Net SDK v9
3. NodeJS (at least version ^20.19.0 || ^22.12.0 || ^24.0.0) - Optional if you want to run the Angular app separately in development mode
4. Clone the project in a User folder on your computer by running:

```bash
# you will of course need git installed to run this
git clone https://github.com/TryCatchLearn/skinet.git
cd skinet
```

5. Restore the packages by running:

```bash
# From the solution folder (skinet)
dotnet restore

# Change directory to client to run the npm install.  Only necessary if you want to run
# the angular app in development mode using the angular dev server
cd client
npm install
```

6. Most of the functionality will work without Stripe but if you wish to see the payment functionality working too then you will need to create a Stripe account and populate the keys from Stripe.  In the API folder create a file called ‘appsettings.json’ with the following code:

```json
{
    "Logging": {
      "LogLevel": {
        "Default": "Information",
        "Microsoft.AspNetCore": "Warning"
      }
    },
    "StripeSettings": {
      "PublishableKey": "pk_test_REPLACEME",
      "SecretKey": "sk_test_REPLACEME",
      "WhSecret": "whsec_REPLACEME"
    },
    "AllowedHosts": "*"
  }
```

7. To use the Stripe webhook you will also need to use the StripeCLI, and when you run this you will be given a whsec key which you will need to add to the appsettings.json.   To get this key and instructions on how to install the Stripe CLI you can go to your Stripe dashboad ⇒ Developers ⇒ Webhooks ⇒ Add local listener.   The whsec key will be visible in the terminal when you run Stripe.
8. Once you have the Stripe CLI you can then run this so it listens to stripe events and forward them to the .Net API:

```bash
#login to stripe
stripe login

# listen to stripe events and forward them to the API
stripe listen --forward-to https://localhost:5001/api/payments/webhook -e payment_intent.succeeded
```

9. The app uses both Sql Server and Redis.   To start these services then you need to run this command from the solution folder.  These are both configured to run on their default ports so ensure you do not have a conflicting DB server running on either port 1433 or port 6379 on your computer:

```bash
# in the skinet folder (root directory of the app)
docker compose up -d 
```

10. You can then run the app and browse to it locally by running:

```bash
# run this from the API folder
cd API
dotnet run
```

11. You can then browse to the app on https://localhost:5001
12. If you wish to run the Angular app in development mode you will need to install a self signed SSL certificate.  The client app is using an SSL certificate generated by mkcert.   To allow this to work on your computer you will need to first install mkcert using the instructions provided in its repo [here](https://github.com/FiloSottile/mkcert), then run the following command:

```bash
# cd into the client ssl folder
cd client/ssl
mkcert localhost
```

13. You can then run both the .Net app and the client app.

```bash
# terminal tab 1
cd API
dotnet run

# terminal tab 2
ng serve
```

===
If you are looking for the repository for the version of this app created on .Net 8.0 and Angular v18 then this is available here:

https://github.com/TryCatchLearn/Skinet-2024