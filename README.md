# Introduction to Commission Microservice

Commission Microservice \(CM\) helps Galileo GDS users to consistently collect commission for their reservations. It transfers responsibility for choosing the right commission rate from reservation handlers to a machine.

## Ways of using

There are three ways CM can help you with your commissions.

* CM Online calculator - a web page for calculating the commission.
* CM API - a service for inserting the commission into the reservation.
* CM Smartpoint plugin - a tool for storing the commission into the booking file.

# Getting started

1. Contact martin.brandysky@travelportgds.cz
2. Get your authentication token for Online calculator and API access or a Smartpoint plugin
3. Define your commission rules - through our Backoffice application or through API
4. Start calculating and storing your commission rates

## Examples

### Definition of the commission in CM Backoffice

![](/assets/commission-example.png)

### CM Online calculator

The Online calculator is a simple page where you can easily test your access Token with a real reservation. You can also use it for calculations.

![](/assets/calculator.png)

The URL is [https://cm.golibe.com/](https://cm.golibe.com/) .

### CM API

The API is running on the UR [https://cm.services.golibe.com/prod/](https://cm.services.golibe.com/prod/). The methods are described on [separate page](/methods.md).

### CM Smartpoint plugin

CM Smartpoint plugin is in a development. It supports calculating and storing commission from Travelport Smartpoint application.

## 



