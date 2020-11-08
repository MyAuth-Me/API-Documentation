# API Documentation
The encryption API is used in the C++ library and you would need to purchase to gain access

## Authenticating
You can send a GET request with these queries to get a response
* Token (This is the license key)
* Hash (This is the users hardware id)
* Secret (This is the public key of your application)

And it will return with one of the following messages.
Quick note: Information is split by `|` so you can split and check the index 0 in the array to check if it's `true` or `error` like shown in the example of authentication
* `false|no_exist` (The license doesn't exist)
* `false|hwid` (The HWID mismatches the database)
* `false|expired` (The license has expired)
* `false|restart` (A value of the license has updated HWID, Expiry. Restart the application or resend request)

## APIs / Server-side requests
It has come to our attention that some people have APIs or other endpoints they have API keys they don't want user to be able to catch through web traffic so we added a API option.
The `call_url.php` endpoint finds the API with the specified public_key and sends a request to it if license is valid
`https://myauth.me/api/call_url.php?secret=YOUR_PUBLIC_KEY&key=LICENSE_KEY&hash=HWID&args=arguments|arguments`

These arguments are treated as query arguments on the uri so if I input
`args=ip=1.1.1.1|port=323`
The end URI would be `api.example.com/info.php?ip=1.1.1.1&port=323` where the web-server then returns the returned data

## Basic Integrity Check
You can check the intergrity of the website by hashing the current UTC time without seconds in this format `Year-month-day hour:minute` and hashing it with md5 and then testing if it matches https://myauth.me/api/verify.php
