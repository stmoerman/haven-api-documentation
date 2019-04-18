---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://www.scopeweb.nyc' target='_blank'>Powered by scopeweb.nyc</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Haven API! You can use our API to access Haven API, which can retrieve and add information to the Havens, Hangouts, Chats and other endpoints.

We have language bindings in Shell! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right in the future when other language bindings become availabe.

If you're interested in the Haven app, visit the [Haven website](https://havenapp.global/), download the app for Android or iOS and get started right away.

# JSON Web Tokens

> To authorize, use this code:


```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Bearer jsonwebtoken"
```

> Make sure to replace `jsonwebtoken` with your API key.

Haven uses JSON web token to allow access to the API. You can retrieve a JWT by logging a user in against our login endpoint.

Haven expects for the JSON web token to be included in all API requests - except for the Auth ones - to the server in a header that looks like the following:

`Authorization: Bearer jsonwebtoken`

<aside class="notice">
You must replace <code>jsonwebtoken</code> with your personal JSON web token.
</aside>

# Authentication

## User login

```shell
curl -X POST \
  "https://api.havenapp.global/v1/users/login" \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'email=your@email.com&password=password'
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "token": "Bearer eyJhbGciOiJ....."
}
```

This endpoint retrieves all kittens.

### HTTP Request

`POST https://api.havenapp.global/v1/users/login`

### Body Parameters

Parameter | Description |
--------- | ----------- |
email     | your email address used for Haven |
available | your password |

<aside class="success">
Remember — a happy user is an authenticated user!
</aside>

## User registration

```shell
curl -X POST \
  "https://api.havenapp.global/v1/users/register" \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'email=your@email.com&password=averysafepassword&username=coolestusername'
```

> The above command returns JSON structured like this:

```json
{
    "message": "Registration successful."
}
```

This endpoint creates a Haven user account.

<!-- <aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside> -->

### HTTP Request

`POST https://api.havenapp.global/v1/users/register`

### Body Parameters

Parameter | Description
--------- | -----------
email     | The email address you wish to register with
password  | The password for your user account
username  | A username for your user account

# User

## Retrieve current user

```shell
curl -X GET \
  https://api.havenapp.global/v1/users/current \
  -H 'Authorization: Bearer jsonwetoken'
```

> The above command returns JSON structured like this:

```json
{
    "avatar": "https://i.imgur.com/kpSxSxm.jpg",
    "role": "user",
    "active": true,
    "_id": "5cb258e8ee4c175e5b348fbe",
    "username": "bobby78",
    "name": "Bob Plumb",
    "lastLogin": "2019-04-13T21:47:20.511Z",
    "tagline": "Truckin'\n#workaholic #country #coors"
}
```

This endpoint retrieves the data belonging to the user that is matching the JSON web token. This route automatically checks who the currently authenticated user is, and sends them back their personal data.

### HTTP Request

`GET https://api.havenapp.global/v1/users/:user_id`

## Retrieve user by id

```shell
curl -X GET \
  https://api.havenapp.global/v1/users/:user_id \
  -H 'Authorization: Bearer jsonwetoken'
```

> The above command returns JSON structured like this:

```json
{
    "avatar": "https://i.imgur.com/kpSxSxm.jpg",
    "role": "user",
    "active": true,
    "_id": "5cb22bd4ee4c175e5b348fb7",
    "username": "tech_lead",
    "name": "The Tech Lead",
    "lastLogin": "2019-04-13T18:35:00.318Z",
    "tagline": "I am the Tech Lead on The Tech Lead. 💻"
}
```

This endpoint retrieves the data belonging to the user that is matching the specified user_id.

### HTTP Request

`GET https://api.havenapp.global/v1/users/:user_id`

### Query Parameters

Parameter | Description
--------- | -----------
user_id   | The id for the User you're trying to retrieve information about

## Update a user's details

```shell
curl -X PUT \
  https://api.havenapp.global/v1/users/:user_id \
  -H 'Authorization: Bearer jsonwetoken'
```

> The above command returns JSON structured like this:

```json
{
    "avatar": "https://i.imgur.com/kpSxSxm.jpg",
    "role": "user",
    "active": true,
    "_id": "5cb22bd4ee4c175e5b348fb7",
    "username": "tech_lead",
    "name": "The Tech Lead",
    "lastLogin": "2019-04-13T18:35:00.318Z",
    "tagline": "I am the Tech Lead on The Tech Lead. 💻"
}
```

This endpoint updates the data belonging to the user that is matching the specified user_id. For this endpoint it is not necessary to provide all the fields, as it will only update the field that you pass along in the body params.

### HTTP Request

`PUT https://api.havenapp.global/v1/users/:user_id`

### Query Parameters

Parameter | Description
--------- | -----------
user_id   | The id for the User you're trying to retrieve information about

### Body Parameters

Parameter | Description
--------- | -----------
username  | The id for the User you're trying to retrieve information about
name      | Updated name for the user
email     | Updated email for the user
avatar    | Updated avatar for the user in Base64 format
tagline   | Updated tagline for the user

# Haven

## Create a new Haven

```shell
curl -X POST \
  "https://api.havenapp.global/v1/haven/" \
  -H 'Authorization: Bearer jsonwetoken' \
  -F 'name=Apple' \
  -F 'tagline=Scamming the world, one repair at a time' \
  -F 'image=base64imagestring'
```

> The above command returns JSON structured like this:

```json
{
    "message": "Haven was succesfully created",
    "haven": {
        "members": [
            "5cb60f40ee4c175e5b348fc4"
        ],
        "owners": [
            "5cb60f40ee4c175e5b348fc4"
        ],
        "_id": "5cb8560c07465801eb05b1bc",
        "name": "Apple loyalists",
        "tagline": "🐑 Follow the herd!",
        "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1555584522/sl2gvx6c9prxl5ijxwp1.png",
        "createdAt": "2019-04-18T10:48:44.436Z",
        "updatedAt": "2019-04-18T10:48:44.436Z",
        "__v": 0
    }
}
```

This endpoint retrieves all kittens.

### HTTP Request

`POST https://api.havenapp.global/v1/users/login`

### Body Parameters

Parameter | Description |
--------- | ----------- |
dataURI   | Base64 image string for the Haven banner |
name      | The name for your Haven |
tagline   | The tagline (description) for your Haven, catchy please! |

## Delete a haven (as owner)

```shell
 curl -X DELETE \
  "https://api.havenapp.global/v1/haven/:haven_id" \
  -H 'Authorization: Bearer jsonwetoken'
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "msg": "Your Haven was succesfully deleted."
}
```

This endpoint deletes a Haven that you're the owner of.

<!-- <aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside> -->

### HTTP Request

`DELETE https://api.havenapp.global/v1/haven/:haven_id`

### Query Parameters

Parameter | Description
--------- | -----------
haven_id  | The id for the Haven you want to delete (as the owner of the Haven)


## Get a Haven by id

```shell
curl -X GET \
  'https://api.havenapp.global/v1/haven/:haven_id' \
  -H 'Authorization: Bearer jsonwetoken'
```

> The above command returns JSON structured like this:

```json
{
    "members": [
        {
            "avatar": "https://res.cloudinary.com/scope-web-llc/image/upload/v1554911632/haven/avatars/m7fgxp6odiyaqdmqhxpw.jpg",
            "_id": "5c8fee7a6b8e37f8cf4096b5",
            "username": "bob"
        }
    ],
    "owners": [
        {
            "avatar": "https://res.cloudinary.com/scope-web-llc/image/upload/v1554911632/haven/avatars/m7fgxp6odiyaqdmqhxpw.jpg",
            "_id": "5c8fee7a6b8e37f8cf4096b5",
            "username": "bob"
        }
    ],
    "_id": "5cb087a067e521fdc6f9bba0",
    "name": "Bob's burgers 🍔",
    "tagline": "More pickles, more onions, no cheese!",
    "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1555072928/haven/havens/banners/obzqo8fszvgrzmwkrj6x.jpg"
}
```

This endpoint retrieves date off one specific Haven, such as the members, owners, name, tagline and banner.

### HTTP Request

`GET https://api.havenapp.global/v1/haven/:haven_id`

### Query Parameters

Parameter | Description
--------- | -----------
haven_id  | The id for the Haven you want to retrieve the information from

## Get all members in a Haven

```shell
curl -X GET \
  'https://api.havenapp.global/v1/haven/:haven_id/members' \
  -H 'Authorization: Bearer jsonwetoken'
```

> The above command returns JSON structured like this:

```json
{
    "_id": "5cb611ded015660d4183722c",
    "members": [
        {
            "_id": "5cb60f40ee4c175e5b348fc4",
            "avatar": "https://i.imgur.com/kpSxSxm.jpg",
            "username": "stephan"
        },
        {
            "_id": "5cb21c34ee4c175e5b348fb5",
            "avatar": "https://i.imgur.com/kpSxSxm.jpg",
            "username": "pojo"
        },
        {
            "_id": "5cb2301bee4c175e5b348fbc",
            "avatar": "https://i.imgur.com/kpSxSxm.jpg",
            "username": "skywalker"
        },
        {
            "_id": "5cb25c12ee4c175e5b348fc2",
            "avatar": "https://i.imgur.com/kpSxSxm.jpg",
            "username": "puppies.are.love"
        }
    ],
    "name": "C++",
    "tagline": "Software for world changers",
    "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1555435998/haven/havens/banners/d9xq1udw9508wzlwffrs.jpg"
}
```

This endpoint retrieves date off one specific Haven, such as the members, owners, name, tagline and banner.

### HTTP Request

`GET https://api.havenapp.global/v1/haven/:haven_id/`

### Query Parameters

Parameter | Description
--------- | -----------
haven_id  | The id for the Haven you want to retrieve the information from

## Join a Haven by id

```shell
curl -X POST \
  'https://api.havenapp.global/v1/haven/:haven_id' \
  -H 'Authorization: Bearer jsonwetoken'
```

> The above command returns JSON structured like this:

```json
{
    "message": "Succesfully joined the Haven"
}
```

This endpoint retrieves date off one specific Haven, such as the members, owners, name, tagline and banner.

### HTTP Request

`POST https://api.havenapp.global/v1/haven/:haven_id`

### Query Parameters

Parameter | Description
--------- | -----------
haven_id  | The id for the Haven you want to join

## List all available Havens

```shell
curl -X GET \
  'https://api.havenapp.global/v1/haven/' \
  -H 'Authorization: Bearer jsonwetoken'
```

> The above command returns JSON structured like this:

```json
{
    "havens": [
        {
            "_id": "5cb087a067e521fdc6f9bba0",
            "members": [
                {
                    "_id": "5c8fee7a6b8e37f8cf4096b5",
                    "avatar": "https://res.cloudinary.com/scope-web-llc/image/upload/v1554911632/haven/avatars/m7fgxp6odiyaqdmqhxpw.jpg",
                    "username": "bob"
                }
            ],
            "owners": [
                {
                    "_id": "5c8fee7a6b8e37f8cf4096b5",
                    "avatar": "https://res.cloudinary.com/scope-web-llc/image/upload/v1554911632/haven/avatars/m7fgxp6odiyaqdmqhxpw.jpg",
                    "username": "bob"
                }
            ],
            "name": "Bob's burgers 🍔",
            "tagline": "More pickles, more onions, no cheese!",
            "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1555072928/haven/havens/banners/obzqo8fszvgrzmwkrj6x.jpg",
            "createdAt": "2019-04-12T12:42:08.467Z",
            "updatedAt": "2019-04-17T09:55:08.062Z",
            "__v": 0
        },
        {
            "_id": "5cb618e5e97faa77b9ae495f",
            "members": [
                {
                    "_id": "5c8fee7a6b8e37f8cf4096b5",
                    "avatar": "https://res.cloudinary.com/scope-web-llc/image/upload/v1554911632/haven/avatars/m7fgxp6odiyaqdmqhxpw.jpg",
                    "username": "bob"
                }
            ],
            "owners": [
                {
                    "_id": "5c8fee7a6b8e37f8cf4096b5",
                    "avatar": "https://res.cloudinary.com/scope-web-llc/image/upload/v1554911632/haven/avatars/m7fgxp6odiyaqdmqhxpw.jpg",
                    "username": "bob"
                }
            ],
            "name": "Stephans Haven",
            "tagline": "Code, code, code your bloat, gently down the stream.",
            "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1555437797/uc2var1r8mjv0ll0onfg.jpg",
            "createdAt": "2019-04-16T18:03:17.618Z",
            "updatedAt": "2019-04-16T18:03:17.618Z",
            "__v": 0
        }
    ]
}
```

This endpoint retrieves date off one specific Haven, such as the members, owners, name, tagline and banner.

### HTTP Request

`GET https://api.havenapp.global/v1/haven/`

### Query Parameters

Parameter | Description
--------- | -----------
haven_id  | The id for the Haven you want to retrieve the information from

## Update a Haven (as owner)

```shell
curl -X PUT \
  'https://api.havenapp.global/v1/haven/:haven_id' \
  -H 'Authorization: Bearer jsonwetoken'
```

> The above command returns JSON structured like this:

```json
{
    "message": "Succesfully joined the Haven"
}
```

This endpoint retrieves date off one specific Haven, such as the members, owners, name, tagline and banner.

### HTTP Request

`PUT https://api.havenapp.global/v1/haven/:haven_id`

### Query Parameters

Parameter | Description
--------- | -----------
haven_id  | The id for the Haven you want to update details off

### Body Parameters

Parameter | Description
--------- | -----------
name      | New name for the Haven
tagline   | New tagline for the Haven
banner    | Base64 image for the Haven
