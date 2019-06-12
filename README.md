# API Documentation for Project Sari

Project Sari is a platform that allows mom-and-pop retailers (Sari Sari Stores) to order groceries goods from vendors ( wholesellers) such as supermarkets and distributors directly.  This document explains how client application can connect to the platform, access vendor product list and submit orders.

## API End Point

* The API is development using REST approach.  
* Current API end point: https://project-sari/heroku.com/c/api
* Example: To get list of available vendors https://project-sari/heroku.com/c/api/vendors/


## Signup

To sign up using API.

**POST** /auth/customer/signup

``` javacript
{ "email": "sample-email@test.com",
  "password": "password"
}
```

Sample result

``` javascript
{ "id": "123421341234234"
  "email": "sample-email@test.com",
  "token": "xxxx"
}
```

## Login

**POST** /auth/customer/login

``` javascript
{ "email": "sample-email@test.com",
  "password": "password"
}
``` 

Sample results:

``` javascript
{ token: "xxxxx"
}
```

## Security

All subseqent calls needs to have token in the header.

```javascript
header: {
  Authentication: "Bearer {token}"
}
```

## Vendors List

To get a list of vendors.

**GET** /vendors


## Product List

To get list of products from a particular vendor.

**GET** /products?vendor_id=XXX


## Order Submission

To send an order to the platform

**POST** /orders

``` javascript
{ "reference": "1",
  [ { "product_id": "1111",
      "price: 100.00,
      "quantity", 1
    } 
  ],  
}
```








