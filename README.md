# Client for AWS AppSync secured with Cognito

This is an example how an AWS AppSync API secured with Cognito as Default authorization mode can be called from clients.

## Setup

The client can be used in two different modes: Basic and Extended.

In Basic mode, the App client needs to be created without a client secret and with "OAuth grant types" as "Implicit grant" (in this mode, the tokens will be directly passed to the client as URL parameters), in Extended mode the App client gets a client secret and the "OAuth grant types" as "Authorization code grant". In both cases set the "Allowed callback URLs" to http://localhost:8080. Create an AppSync API secured with that Cognito user pool. Ensure access to the AppSync API is only possible via Cognito through the AWS console by running an example query that should return in `Request failed with status code 401` and only after "Login with User Pools" displays a valid response. Please note that in Extended mode the AppSync UI won't be able to log in to Cognito due to its missing client secret implementation.

Edit the respective `index.html` and replace the Cognito URL, Cognito client ID, Cognito client secret (if using Extended mode), AppSync URL, AppSync query and (if using Extended mode) the hosts in `nginx.conf` with the respective values from Cognito and AppSync.

## Demo

For Basic mode, run the client via `(cd basic && docker run -p 8080:80 -v ./index.html:/usr/share/nginx/html/index.html nginx)`, for Extended mode, run the client via `(cd extended && docker-compose up --build)`. Open http://localhost:8080 in your Browser: Initially, the client redirects to Cognito where the user authenticates and then gets redirected back to the client which either uses the token directly (Basic) or exchanges the code against the token (Extended) to then call AppSync and displays the result. To repeat the process, clear the `localStorage` of your Browser holding the Cognito ID token to verify a user is already authenticated or not or wait until the token expires (1 hour by default, can be configured in Cognito).