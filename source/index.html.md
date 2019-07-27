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
curl 'api_endpoint_here'
  -H 'Authorization: Bearer jsonwebtoken'
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
  'https://api.havenapp.global/v1/users/login' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'email=your@email.com&password=password&registrationToken=registrationToken'
```

> The above command returns JSON structured like this:

```json
{
  "token": "Bearer jsonwebtoken",
  "refreshToken": "jsonwebtoken",
  "expiresIn": "2019-05-25T14:15:35.134Z",
  "status": 200
}
```

This endpoint performs a login authentication request and returns JWT, expiresIn date for the token, and a 200 status.

### HTTP Request

`POST https://api.havenapp.global/v1/users/login`

### Body Parameters

| Parameter | Description                       |
| --------- | --------------------------------- |
| email     | your email address used for Haven |
| password  | your password                     |
| registrationToken | Google FCM registration token |

<aside class="success">
Remember — a happy user is an authenticated user!
</aside>

## User registration

```shell
curl -X POST \
  'https://api.havenapp.global/v1/users/register' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'email=your@email.com&password=averysafepassword&username=coolestusername'
```

> The above command returns JSON structured like this:

```json
{
  "message": "Registration successful.",
  "status": 200
}
```

This endpoint creates a Haven user account.

<!-- <aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside> -->

### HTTP Request

`POST https://api.havenapp.global/v1/users/register`

### Body Parameters

| Parameter | Description                                 |
| --------- | ------------------------------------------- |
| email     | The email address you wish to register with |
| password  | The password for your user account          |
| username  | A username for your user account            |

## Refresh token

```shell
curl -X POST \
  https://api.havenapp.global/v1/users/verify-token \
  -d refreshToken=yourrefreshtokenhere
```

> The above command returns JSON structured like this:

```json
{
  "token": "Bearer jsonwebtokenhere",
  "expiresIn": "2019-05-25T14:14:23.552Z",
  "status": 200
}
```

This endpoint refreshes your JWT when it expired.

### HTTP Request

`POST https://api.havenapp.global/v1/users/verify-token`

### Body Parameters

| Parameter    | Description                                                                |
| ------------ | -------------------------------------------------------------------------- |
| refreshToken | The refresh token obtained during login, allowing you to recieve a new JWT |

<aside class="success">
Remember — a happy user is an refreshed user!
</aside>

## Reset password request

```shell
curl -X POST \
  'https://api.havenapp.global/v1/users/forgot-password' \
  -d 'email=your@email.com'
```

> The above command returns JSON structured like this:

```json
{
  "success": "An e-mail has been sent to your@email.com with further instructions.",
  "status": 200
}
```

This endpoint sends a password reset token to the users email address, allowing them to regain access to their account

### HTTP Request

`POST https://api.havenapp.global/v1/users/forgot-password`

### Body Parameters

| Parameter | Description                       |
| --------- | --------------------------------- |
| email     | Your email address used for Haven |

# User

## Retrieve current user

```shell
curl -X GET \
  'https://api.havenapp.global/v1/users/current' \
  -H 'Authorization: Bearer jsonwebtoken'
```

> The above command returns JSON structured like this:

```json
{
  "user": {
      "avatar": "https://res.cloudinary.com/scope-web-llc/image/upload/v1560257979/haven/users/avatar/jason.jpg",
      "role": "user",
      "hangoutCount": 0,
      "huddleCount": 0,
      "banned": false,
      "_id": "5cff7e05b2ef9817982df13b",
      "username": "jason",
      "email": "jason@jason.com",
      "lastLogin": "2019-06-11T12:36:18.615Z",
      "tagline": ""
  },
  "status": 200
}
```

This endpoint retrieves the data belonging to the user that is matching the JSON web token. This route automatically checks who the currently authenticated user is, and sends them back their personal data.

### HTTP Request

`GET https://api.havenapp.global/v1/users/current`

## Retrieve user by id

```shell
curl -X GET \
  'https://api.havenapp.global/v1/users/:user_id' \
  -H 'Authorization: Bearer jsonwebtoken'
```

> The above command returns JSON structured like this:

```json
{
  "user": {
    "avatar": "https://res.cloudinary.com/scope-web-llc/image/upload/v1560257979/haven/users/avatar/jason.jpg",
    "role": "user",
    "hangoutCount": 0,
    "huddleCount": 0,
    "banned": false,
    "_id": "5cff7e05b2ef9817982df13b",
    "username": "jason",
    "name": "Jason",
    "lastLogin": "2019-06-13T14:22:13.833Z",
    "location": "San Francisco, CA",
    "tagline": ""
  },
  "status": 200
}
```

This endpoint retrieves the data belonging to the user that is matching the specified user_id.

### HTTP Request

`GET https://api.havenapp.global/v1/users/:user_id`

### Query Parameters

| Parameter | Description                                                     |
| --------- | --------------------------------------------------------------- |
| user_id   | The id for the User you're trying to retrieve information about |

## Update your user account details

```shell
curl -X PUT \
  'https://api.havenapp.global/v1/users/:user_id' \
  -H 'Authorization: Bearer jsonwebtoken'
```

> The above command returns JSON structured like this:

```json
{
  "message": "Succesfully updated user profile details.",
  "user": {
    "avatar": "https://res.cloudinary.com/scope-web-llc/image/upload/v1560437447/haven/users/avatar/jason.jpg",
    "role": "user",
    "hangoutCount": 0,
    "huddleCount": 0,
    "banned": false,
    "_id": "5cff7e05b2ef9817982df13b",
    "username": "jason",
    "email": "jason@jason.com",
    "age": "2002-05-16T12:50:47.072Z",
    "lastLogin": "2019-06-13T14:22:13.833Z",
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjVjZmY3ZTA1YjJlZjk4MTc5ODJkZjEzYiIsInVzZXJuYW1lIjoiamFzb24iLCJhdmF0YXIiOiJodHRwczovL3Jlcy5jbG91ZGluYXJ5LmNvbS9zY29wZS13ZWItbGxjL2ltYWdlL3VwbG9hZC92MTU2MDI1Nzk3OS9oYXZlbi91c2Vycy9hdmF0YXIvamFzb24uanBnIiwicm9sZSI6InVzZXIiLCJpYXQiOjE1NjA0MzcxMDMsImV4cCI6MTU5MTk5NDcwM30.DHAE-4AJVXwqbozZ8uK2ZkryzDUAGjont5Hw5Neu58k",
    "tagline": "",
    "name": "Jason",
    "location": "San Francisco, CA",
    "updatedAt": "2019-06-13T14:50:47.624Z"
  },
  "status": 200
}
```

This endpoint updates the data belonging to the user that is matching the specified user_id. For this endpoint it is **not** necessary to provide all the fields, as it will only update the field that you pass along in the body params.

### HTTP Request

`PUT https://api.havenapp.global/v1/users/:user_id`

### Query Parameters

| Parameter | Description               |
| --------- | ------------------------- |
| user_id   | Your user account user_id |

### Body Parameters

| Parameter | Description                                 |
| --------- | ------------------------------------------- |
| username  | Updated username for your user account      |
| name      | Updated name for your user account          |
| email     | Updated email for your user account         |
| location  | Updated location for your user account      |
| tagline   | Updated tagline for your user account       |
| avatar    | Updated base64 avatar for your user account |

## List a users Havens

```shell
curl -X GET \
  'https://api.havenapp.global/v1/users/:user_id/havens' \
  -H 'Authorization: Bearer jsonwebtoken'
```

> The above command returns JSON structured like this:

```json
{
  "havens": [
    {
      "huddles": 0,
      "admins": [],
      "_id": "5cf81c3ae5418a20ded7887e",
      "name": "Test Haven",
      "tagline": "tagline for days",
      "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1559764025/haven/havens/banners/xrbrjxfirbnhwrcwrhhy.jpg"
    },
    {
      "huddles": 0,
      "admins": [],
      "_id": "5cf81c3ae5418a20ded7887f",
      "name": "Test Haven 2",
      "tagline": "tagline for days 2",
      "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1559764025/haven/havens/banners/xrbrjxfirbnhwrcwrhhy.jpg"
    }
  ],
  "status": 200
}
```

This endpoint finds the Haven's of which a user is a member or owner. It returns each Haven's id, name, tagline and banner.

### HTTP Request

`GET https://api.havenapp.global/v1/users/:user_id/havens`

### Query Parameters

| Parameter | Description                                                     |
| --------- | --------------------------------------------------------------- |
| user_id   | The id for the User you're trying to retrieve information about |

## Delete a user's account

```shell
curl -X DELETE \
  'https://api.havenapp.global/v1/users/:user_id' \
  -H 'Authorization: Bearer jsonwebtoken'
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "msg": "Your account was succesfully deleted.",
  "status": 200
}
```

This endpoint deletes the user account for the provided user_id param, as long as it matches the user_id of the requester. All data belonging to the user is automatically purged and deleted.

### HTTP Request

`PUT https://api.havenapp.global/v1/users/:user_id`

### Query Parameters

| Parameter | Description               |
| --------- | ------------------------- |
| user_id   | Your user account user_id |

## Update user's location

```shell
curl -X PUT \
  'https://api.havenapp.global/v1/users/status/update_location' \
  -H 'Authorization: Bearer jsonwebtoken'
  -d current_location=12345
```

> The above command returns JSON structured like this:

```json
{
  "message": "Succesfully updated your location.",
  "current_location": "12345",
  "status": 200
}
```

This endpoint updates the data belonging to the user that is matching the specified user_id. For this endpoint it is **not** necessary to provide all the fields, as it will only update the field that you pass along in the body params.

### HTTP Request

`PUT https://api.havenapp.global/v1/users/status/update_location`

### Body Parameters

| Parameter        | Description                                    |
| ---------------- | ---------------------------------------------- |
| current_location | Updated current location for your user account |


# Haven

## Create a new Haven

```shell
curl -X POST \
  'https://api.havenapp.global/v1/haven/' \
  -H 'Authorization: Bearer jsonwebtoken' \
  -F 'name=Apple' \
  -F 'tagline=Scamming the world, one repair at a time' \
  -F 'image=base64imagestring'
```

> The above command returns JSON structured like this:

```json
{
  "message": "Haven was succesfully created",
  "haven": {
    "members": ["5cb60f40ee4c175e5b348fc4"],
    "owners": ["5cb60f40ee4c175e5b348fc4"],
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

This endpoint creates a new Haven.

### HTTP Request

`POST https://api.havenapp.global/v1/haven/`

### Body Parameters

| Parameter | Description                                              |
| --------- | -------------------------------------------------------- |
| dataURI   | Base64 image string for the Haven banner                 |
| name      | The name for your Haven                                  |
| tagline   | The tagline (description) for your Haven, catchy please! |

## Delete a haven (as owner)

```shell
 curl -X DELETE \
  'https://api.havenapp.global/v1/haven/:haven_id' \
  -H 'Authorization: Bearer jsonwebtoken'
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

| Parameter | Description                                                         |
| --------- | ------------------------------------------------------------------- |
| haven_id  | The id for the Haven you want to delete (as the owner of the Haven) |

## Get Haven by id

```shell
curl -X GET \
  'https://api.havenapp.global/v1/haven/:haven_id' \
  -H 'Authorization: Bearer jsonwebtoken'
```

> The above command returns JSON structured like this:

```json
{
    "haven": {
        "huddles": 0,
        "admins": [],
        "owners": [
            {
                "avatar": "https://api.havenapp.global/images/avatar.png",
                "hangoutCount": 0,
                "huddleCount": 0,
                "_id": "5cff9614ffbb29d1b130a194",
                "username": "redangel",
                "name": "redangel"
            }
        ],
        "_id": "5d239616cb9462bc5df00a36",
        "name": "YAYEEETTTT",
        "tagline": "Rage gaming at it's finest",
        "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1562613270/haven/havens/banners/YAYEEETTTT.jpg"
    },
    "isMember": true,
    "status": 200
}
```

This endpoint retrieves date off one specific Haven, such as the members, owners, name, tagline and banner.

### HTTP Request

`GET https://api.havenapp.global/v1/haven/:haven_id`

### Query Parameters

| Parameter | Description                                                    |
| --------- | -------------------------------------------------------------- |
| haven_id  | The id for the Haven you want to retrieve the information from |

## Get all members in a Haven

```shell
curl -X GET \
  'https://api.havenapp.global/v1/haven/:haven_id/members' \
  -H 'Authorization: Bearer jsonwebtoken'
  -F pageNo=2 \
  -F size=10
```

> The above command returns JSON structured like this:

```json
{
  "haven": {
    "members": [
      {
        "avatar": "https://api.havenapp.global/images/avatar.png",
        "hangoutCount": 0,
        "huddleCount": 0,
        "_id": "5cff9614ffbb29d1b130a194",
        "username": "redangel",
        "name": "redangel"
      },
      {
        "avatar": "https://api.havenapp.global/images/avatar.png",
        "hangoutCount": 0,
        "huddleCount": 0,
        "_id": "5d13c0291e89e4b212c5dc43",
        "username": "stephan"
      }
    ],
    "_id": "5d239616cb9462bc5df00a36"
  },
  "memberCount": 2,
  "status": 200
}
```

This endpoint retrieves date off one specific Haven, such as the members, owners, name, tagline and banner.

### HTTP Request

`GET https://api.havenapp.global/v1/haven/:haven_id/members`

### Query Parameters

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| haven_id  | The id for the Haven you want to retrieve the members from   |
| pageNo    | Page number to load more members (pagination)                |
| size      | The amount of members you want to load per page (pagination) |

## Join a Haven by id

```shell
curl -X POST \
  'https://api.havenapp.global/v1/haven/:haven_id' \
  -H 'Authorization: Bearer jsonwebtoken'
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

| Parameter | Description                           |
| --------- | ------------------------------------- |
| haven_id  | The id for the Haven you want to join |

## Join a Haven using joinCode

```shell
curl -X POST \
  'https://api.havenapp.global/v1/haven/referral/:join_code' \
  -H 'Authorization: Bearer jsonwebtoken'
```

> The above command returns JSON structured like this:

```json
{
  "message": "Succesfully joined the Haven"
}
```

This endpoint retrieves date off one specific Haven, such as the members, owners, name, tagline and banner.

### HTTP Request

`POST https://api.havenapp.global/v1/haven/referral/:join_code`

### Query Parameters

| Parameter | Description                           |
| --------- | ------------------------------------- |
| join_code | The join code provided to you by admin |

## List all available Havens

```shell
curl -X GET \
  'https://api.havenapp.global/v1/haven/' \
  -H 'Authorization: Bearer jsonwebtoken'
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

| Parameter | Description                                                    |
| --------- | -------------------------------------------------------------- |
| haven_id  | The id for the Haven you want to retrieve the information from |

## Update a Haven (as owner)

```shell
curl -X PUT \
  'https://api.havenapp.global/v1/haven/:haven_id' \
  -H 'Authorization: Bearer jsonwebtoken'
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

| Parameter | Description                                         |
| --------- | --------------------------------------------------- |
| haven_id  | The id for the Haven you want to update details off |

### Body Parameters

| Parameter | Description                |
| --------- | -------------------------- |
| name      | New name for the Haven     |
| tagline   | New tagline for the Haven  |
| dataURI   | Base64 image for the Haven |

## Post an announcement (as owner or admin)

```shell
curl -X POST \
  https://api.havenapp.global/v1/haven/:haven_id/announcement \
  -H 'Authorization: Bearer jsonwebtoken' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'title=First%20announcement&description=your announcement description&dataURI=ibase64imagestring&location=zipcode'
```

> The above command returns JSON structured like this:

```json
{
  "message": "Succesfully posted the announcement.",
  "announcement": {
    "_id": "5cda8b1578d47c2238850fe7",
    "title": "First announcement",
    "description": "This announcement is the first one to ever touch this database, isn't that amazing?",
    "image": "https://res.cloudinary.com/scope-web-llc/image/upload/v1557826323/haven/havens/posts/pul1zvzjb6ebc66hb7q1.png",
    "date": "2019-05-14T09:32:05.257Z",
    "location": "10030",
    "havenId": "5cb087a067e521fdc6f9bba0",
    "sender": "5c8fee8c6b8e37f8cf4096b6",
    "createdAt": "2019-05-14T09:32:05.262Z",
    "updatedAt": "2019-05-14T09:32:05.262Z",
    "__v": 0
  },
  "status": 200
}
```

This endpoint allows owners and admins to post an announcement to a Haven.

### HTTP Request

`POST https://api.havenapp.global/v1/haven/:haven_id/announcement`

### Query Parameters

| Parameter | Description                                               |
| --------- | --------------------------------------------------------- |
| haven_id  | The id for the Haven you want to send the announcement to |

### Body Parameters

| Parameter       | Description                           |
| --------------- | ------------------------------------- |
| title           | The title for the announcement        |
| description     | Description text for the announcement |
| dataURI         | Base64 image for the announcement     |
| data (optional) | Optional data for the announcement    |
| location        | Location for the announcement         |

## Retrieve joinCode (as owner or admin)

```shell
curl -X GET \
  https://api.havenapp.global/v1/haven/:haven_id/joinCode \
  -H 'Authorization: Bearer jsonwebtoken' \
```

> The above command returns JSON structured like this:

```json
{
  "success": "Retrieved joinCode",
  "joinCode": "1a2b3c4dbe",
  "status": 200
}
```

This endpoint allows owners and admins to retrieve a Haven's joinCode.

### HTTP Request

`GET https://api.havenapp.global/v1/haven/:haven_id/joinCode`

### Query Parameters

| Parameter | Description                                               |
| --------- | --------------------------------------------------------- |
| haven_id  | The id for the Haven you want to retrieve the code from   |

# Hangout

## Create a new post

```shell
curl -X POST \
  'https://api.havenapp.global/v1/hangout/:haven_id/post' \
  -H 'Authorization: Bearer jsonwebtoken' \
  -d 'title=Posttitle&description=The%20%description%20for%20your%20%post&location=10039&dataURI=base64imagestring
```

> The above command returns JSON structured like this:

```json
{
  "message": "Succesfully created the Hangout post",
  "hangoutId": "5cb8fad18488d6302619b742",
  "conversationId": "5cb8facf8488d6302619b740",
  "status": 200
}
```

This endpoints allows you to create a new post for a Haven you're a member or owner of.

### HTTP Request

`POST https://api.havenapp.global/v1/hangout/:haven_id/post`

### Query Parameters

| Parameter | Description                                   |
| --------- | --------------------------------------------- |
| haven_id  | The id for the Haven you're sending a post to |

### Body Parameters

| Parameter   | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| title       | The title for the post that you want to create               |
| description | The description for the post, shown in the form of a tagline |
| dataURI     | The picture (Base64) that will be used to show in the feed   |
| location    | ZIP code of the user                                         |
| eventDate   | An (optional) event date for the post                        |

## Join a post by id

```shell
curl -X POST \
  'https://api.havenapp.global/v1/hangout/:hangout_id/join' \
  -H 'Authorization: Bearer jsonwebtoken' \
```

> The above command returns JSON structured like this:

```json
{
  "message": "Succesfully created the Hangout post",
  "hangoutId": "5cb8fad18488d6302619b742",
  "conversationId": "5cb8facf8488d6302619b740",
  "status": 200
}
```

This endpoint allows you to join a Hangout as a user, which also makes you a part of the ongoing conversationId.

### HTTP Request

`POST https://api.havenapp.global/v1/hangout/:hangout_id/join`

### Query Parameters

| Parameter  | Description                                                   |
| ---------- | ------------------------------------------------------------- |
| hangout_id | The id for the hangout (post/chat) that you're trying to join |

## Retrieve all posts of a Haven

```shell
curl -X GET \
  'https://api.havenapp.global/v1/hangout/:haven_id/posts' \
  -H 'Authorization: Bearer jsonwebtoken' \
  -F pageNo=2 \
  -F size=10
```

> The above command returns JSON structured like this:

```json
{
  "message": "Succesfully retrieved Hangout posts",
  "Posts": [
    {
      "_id": "5cb65c37c0bb758e69bdf074",
      "title": "Galaxy",
      "description": "For all the black holes out there!",
      "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1555455029/ncphwoxrfrtggpidgkjd.png",
      "location": "12339",
      "conversationId": "5cb65c34c0bb758e69bdf072",
      "havenId": "5cb087a067e521fdc6f9bba0"
    },
    {
      "_id": "5cb65b73587af38c11501d93",
      "title": "Galaxy",
      "description": "For all the black holes out there!",
      "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1555454833/fcszev8tvryaldkwqigc.png",
      "location": "12339",
      "conversationId": "5cb65b6f587af38c11501d91",
      "havenId": "5cb087a067e521fdc6f9bba0"
    },
    {
      "_id": "5cb5e6c8cccbc30d82f0ff53",
      "title": "Post title",
      "description": "Post description",
      "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1555424967/haven/havens/posts/erumcfyrydznjzjyfscw.jpg",
      "location": "10031",
      "conversationId": "5cb5e6c8cccbc30d82f0ff52",
      "havenId": "5cb087a067e521fdc6f9bba0"
    }
  ],
  "status": 200
}
```

This endpoint allows you to retrieve all posts that are part of a Haven (specified by the id).

### HTTP Request

`GET https://api.havenapp.global/v1/hangout/:haven_id/posts`

### Query Parameters

| Parameter | Description                                               |
| --------- | --------------------------------------------------------- |
| haven_id  | The id for the Haven you're trying to retrieve posts from |

### Body Parameters

| Parameter | Description                                                |
| --------- | ---------------------------------------------------------- |
| pageNo    | Page number to load more posts (pagination)                |
| size      | The amount of posts you want to load per page (pagination) |

## Reply to a Hangout conversation

```shell

curl -X POST \
  'http://localhost:5000/v1/hangout/reply/:conversationId' \
  -H 'Authorization: Bearer jsonwebtoken' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'composedMessage=Yoooo'
```

> The above command returns JSON structured like this:

```json
{
  "message": "Reply succesfully sent.",
  "chat": {
    "delivered": false,
    "read": false,
    "_id": "5cb998fc947e5018cc3c1a89",
    "conversationId": "5cb65c34c0bb758e69bdf072",
    "chatMessage": "Yooo Hangout, what's good my dudes?",
    "sender": "5c8fee7a6b8e37f8cf4096b5",
    "hangoutName": "Galaxy",
    "createdAt": "2019-04-19T09:46:36.640Z",
    "updatedAt": "2019-04-19T09:46:36.640Z",
    "__v": 0
  },
  "status": 200
}
```

This endpoint allows you to reply to a conversation that is part of a Hangout.

### HTTP Request

`POST https://api.havenapp.global/v1/hangout/reply/:conversationId`

### Query Parameters

| Parameter       | Description                                    |
| --------------- | ---------------------------------------------- |
| conversationId  | The id for the conversation you're replying to |
| composedMessage | The message a user is sending                  |

### Body Parameters

| Parameter       | Description                   |
| --------------- | ----------------------------- |
| composedMessage | The message a user is sending |

## Delete a Hangout & Conversation

```shell

curl -X DELETE \
  'https://api.havenapp.global/v1/hangout/:hangout_id' \
  -H 'Authorization: Bearer jsonwebtoken' \
```

> The above command returns JSON structured like this:

```json
{
    "success": "Succesfully deleted the Hangout & Conversation",
    "status": 200
}
```

This endpoint allows you to delete a Hangout & the conversation belonging to it.

### HTTP Request

`DELETE https://api.havenapp.global/v1/hangout/:hangout_id`

### Query Parameters

| Parameter  | Description                               |
| ---------- | ----------------------------------------- |
| hangout_id | The id for the Hangout you want to delete |

## Retrieve all posts you own

```shell

curl -X GET \
  'https://api.havenapp.global/v1/hangout/owner' \
  -H 'Authorization: Bearer jsonwebtoken' \
```

> The above command returns JSON structured like this:

```json

{
  "message": "Succesfully retrieved all Hangout posts you are the owner off",
  "posts": [
    {
      "participants": [
        {
          "avatar": "https://api.havenapp.global/images/avatar.png",
          "_id": "5d13c0291e89e4b212c5dc43",
          "username": "stephan"
        }
      ],
      "_id": "5cff9cefffbb29d1b130a19b",
      "title": "Jets for the best!",
      "description": "Whatever lads! Get the jets!",
      "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1560255725/haven/havens/posts/y8ckdlie09hgbjzgyybh.jpg",
      "location": "37.4219983,-122.084",
      "eventDate": "2019-06-11T12:16:00.000Z",
      "conversationId": "5cff9cedffbb29d1b130a19a",
      "havenId": "5cff97aaffbb29d1b130a199",
      "createdAt": "2019-06-11T12:22:07.326Z"
    }
  ],
  "status": 200
}
```

This endpoint allows you to retrieve all Hangout posts you're the owner off.

### HTTP Request

`GET https://api.havenapp.global/v1/hangout/owner`

## Retrieve all posts you've joined

```shell

curl -X GET \
  'https://api.havenapp.global/v1/hangout/joined' \
  -H 'Authorization: Bearer jsonwebtoken' \
```

> The above command returns JSON structured like this:

```json
{
  "message": "Succesfully retrieved all Hangout posts you have joined",
  "posts": [
    {
      "_id": "5cf827a6b4f9493c987c1212",
      "title": "Galaxy",
      "description": "For all the star gazers out there!",
      "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1559766948/haven/havens/posts/vzknpsaxbkjg5bxr4ulb.png",
      "location": "12339",
      "conversationId": "5cf827a3b4f9493c987c1211",
      "havenId": "5cf81c3ae5418a20ded7887e",
      "createdAt": "2019-06-05T20:35:50.320Z"
    },
    {
      "_id": "5d0f7d18f19d100e58e71a9c",
      "title": "Galaxy 2",
      "description": "For all the star gazers out there! 2",
      "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1561296149/haven/havens/posts/bmgmomdrxtk6a6zst28y.png",
      "location": "12339",
      "conversationId": "5d0f7d0df19d100e58e71a9b",
      "havenId": "5cf81c3ae5418a20ded7887e",
      "createdAt": "2019-06-23T13:22:32.320Z"
    },
    {
      "_id": "5d0f7d47f19d100e58e71a9f",
      "title": "Andrews Hangout",
      "description": "Just because Andrew rocks!",
      "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1561296197/haven/havens/posts/v6xrvzopvt6khremizb3.png",
      "location": "12339",
      "conversationId": "5d0f7d43f19d100e58e71a9e",
      "havenId": "5cf81c3ae5418a20ded7887e",
      "createdAt": "2019-06-23T13:23:19.834Z"
    }
  ],
  "status": 200
}
```

This endpoint allows you to retrieve all Hangout posts you've joined.

### HTTP Request

`GET https://api.havenapp.global/v1/hangout/joined`

# Chat

## Start a new private chat

```shell
curl -X POST \
  'https://api.havenapp.global/v1/chat/new/:user_id' \
  -H 'Authorization: Bearer jsonwebtoken' \
  -d 'composedMessage=hifriend
```

> The above command returns JSON structured like this:

```json
{
  "message": "Conversation started!",
  "conversationId": "5cb99a8d5a71af27e8867927",
  "status": 200
}
```

This endpoints allows you to start a new private (one-to-one) chat with another user.

### HTTP Request

`POST https://api.havenapp.global/v1/chat/new/:user_id`

### Query Parameters

| Parameter | Description                                      |
| --------- | ------------------------------------------------ |
| user_id   | The id of the user you're sending the message to |

### Body Parameters

| Parameter       | Description                                                      |
| --------------- | ---------------------------------------------------------------- |
| composedMessage | The message you're sending to the other user in the private chat |

## Reply to a conversation by id

```shell
curl -X POST \
  'https://api.havenapp.global/v1/chat/reply/:conversationId' \
  -H 'Authorization: Bearer jsonwebtoken' \
  -d 'composedMessage=Lmao!&receiver=receiver_user_id'
```

> The above command returns JSON structured like this:

```json
{
  "message": "Reply succesfully sent.",
  "chat": {
    "delivered": false,
    "read": false,
    "_id": "5cba41d66d8b605d8c9bb74a",
    "conversationId": "5cb99a8d5a71af27e8867927",
    "chatMessage": "Ok well Im out pce",
    "sender": "5c8fee7a6b8e37f8cf4096b5",
    "receiver": "5c8fee8c6b8e37f8cf4096b6",
    "createdAt": "2019-04-19T21:47:02.828Z",
    "updatedAt": "2019-04-19T21:47:02.828Z",
    "__v": 0
  },
  "status": 200
}
```

This endpoints allows you to reply to a private (one-to-one) chat with another user.

### HTTP Request

`POST https:///api.havenapp.global/v1/chat/reply/:conversationId`

### Query Parameters

| Parameter | Description                                      |
| --------- | ------------------------------------------------ |
| user_id   | The id of the user you're sending the message to |

### Body Parameters

| Parameter       | Description                                                      |
| --------------- | ---------------------------------------------------------------- |
| composedMessage | The message you're sending to the other user in the private chat |
| receiver        | The user_id for the receiver of the message you are sending      |

## Retrieve a single conversation

```shell
curl -X GET \
  'https://api.havenapp.global/v1/chat/:conversationId' \
  -H 'Authorization: Bearer jsonwebtoken' \
  -d 'composedMessage=Lmao!&receiver=receiver_user_id'
```

> The above command returns JSON structured like this:

```json
{
  "message": "Reply succesfully sent.",
  "status": 200
}
```

This endpoints allows you to retrieve all messages inside a private (one-to-one) chat with another user.

### HTTP Request

`GET https://api.havenapp.global/v1/chat/:conversationId`

### Query Parameters

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| conversationId | The id for the conversation you're getting the messages from |

### Body Parameters

| Parameter | Description                                                   |
| --------- | ------------------------------------------------------------- |
| pageNo    | Page number to load more message (pagination)                 |
| size      | The amount of messages you want to load per page (pagination) |

## Retrieve current user's chats

```shell
curl -X GET \
  'https://api.havenapp.global/v1/chat/' \
  -H 'Authorization: Bearer jsonwebtoken'
```

> The above command returns JSON structured like this:

```json
{
  "message": "Conversations retrieved",
  "conversations": [
    [
      {
        "conversationId": "5cb5d23aff266cdb38c87b72",
        "chatMessage": "Hey Craig",
        "sender": {
          "avatar": "https://res.cloudinary.com/scope-web-llc/image/upload/v1554911632/haven/avatars/m7fgxp6odiyaqdmqhxpw.jpg",
          "_id": "5c8fee7a6b8e37f8cf4096b5",
          "username": "bob"
        },
        "receiver": {
          "avatar": "https://api.havenapp.global/images/avatar.png",
          "_id": "5cb5cfb94262e7c689ef0cbe",
          "username": "craig"
        },
        "createdAt": "2019-04-16T13:01:46.554Z"
      }
    ],
    [
      {
        "conversationId": "5cb5d246ff266cdb38c87b74",
        "chatMessage": "Hey Richard",
        "sender": {
          "avatar": "https://res.cloudinary.com/scope-web-llc/image/upload/v1554911632/haven/avatars/m7fgxp6odiyaqdmqhxpw.jpg",
          "_id": "5c8fee7a6b8e37f8cf4096b5",
          "username": "bob"
        },
        "receiver": {
          "avatar": "https://api.havenapp.global/images/avatar.png",
          "_id": "5cb5cfc14262e7c689ef0cbf",
          "username": "richard"
        },
        "createdAt": "2019-04-16T13:01:58.086Z"
      }
    ],
    [
      {
        "conversationId": "5cb65c34c0bb758e69bdf072",
        "chatMessage": "Yooo Hangout, what's good my dudes?",
        "sender": {
          "avatar": "https://res.cloudinary.com/scope-web-llc/image/upload/v1554911632/haven/avatars/m7fgxp6odiyaqdmqhxpw.jpg",
          "_id": "5c8fee7a6b8e37f8cf4096b5",
          "username": "bob"
        },
        "createdAt": "2019-04-19T09:46:36.640Z"
      }
    ]
  ],
  "status": 200
}
```

This endpoints allows you to retrieve all conversations, showing the most recent message per "thread".

### HTTP Request

`GET https://api.havenapp.global/v1/chat/`

# Feedback

## Send feedback form to admin email

```shell
curl -X POST \
  'https://api.havenapp.global/v1/feedback' \
  -H 'Authorization: Bearer jsonwebtoken' \
  -d 'message=Yourmessagehere'
```

> The above command returns JSON structured like this:

```json
{
  "message": "Success. Feedback email has been sent",
  "status": 200
}
```

This endpoints allows you to send a feedback form which will be dispatched using NodeMailer, through SendGrid, to the admin email.

### HTTP Request

`POST https://api.havenapp.global/v1/feedback`

### Body Parameters

| Parameter | Description                                             |
| --------- | ------------------------------------------------------- |
| message   | The feedback message containing the body of the message |

# Report

## Retrieve all reports

```shell
curl -X GET \
  'https://api.havenapp.global/v1/report' \
  -H 'Authorization: Bearer jsonwebtoken'
  -d 'type=report type'
```

> The above command returns JSON structured like this:

```json
{
  "reports": [
    {
      "_id": "5ce866ba0a003baff90189f6",
      "type": "haven",
      "description": "This Haven is acting up real weird, bunch of guys posting the weirdest stuff. I'd rather hang with the cool kids, you know?",
      "date": "2019-05-24T21:48:42.417Z",
      "havenId": {
        "_id": "5cc8acb35c08630a030bae1f",
        "owners": [
          "5cc719d7cae6c00faaebad34",
          "5cc71e1ea15f3c2bd836f66c",
          "5cc71f18a15f3c2bd836f66e"
        ],
        "name": "Test_Haven"
      },
      "reportedBy": {
        "_id": "5cc71f18a15f3c2bd836f66e",
        "avatar": "https://api.havenapp.global/images/avatar.png",
        "username": "johndoe"
      },
      "createdAt": "2019-05-24T21:48:42.420Z",
      "updatedAt": "2019-05-24T21:48:42.420Z",
      "__v": 0
    }
  ],
  "status": 200
}
```

This endpoint retrieves the reports that have been sent by users, after verifying your Admin role. Optionally, you can specify a type in the request of the body to retrieve the specific reports by type (i.e. haven, hangout, user).

### HTTP Request

`GET https://api.havenapp.global/v1/report`

### Body Parameters

| Parameter | Description                                              |
| --------- | -------------------------------------------------------- |
| type      | (Optional) the type of report you are trying to retrieve |

## Retrieve a Report by report_id

```shell
curl -X GET \
  'https://api.havenapp.global/v1/report/:report_id' \
  -H 'Authorization: Bearer jsonwebtoken'
```

> The above command returns JSON structured like this:

```json
{
  "report": {
    "_id": "5ce866ba0a003baff90189f6",
    "type": "haven",
    "description": "This Haven is acting up real weird, bunch of guys posting the weirdest stuff. I'd rather hang with the cool kids, you know?",
    "date": "2019-05-24T21:48:42.417Z",
    "havenId": {
      "admins": [],
      "owners": [
        "5cc719d7cae6c00faaebad34",
        "5cc71e1ea15f3c2bd836f66c",
        "5cc71f18a15f3c2bd836f66e"
      ],
      "_id": "5cc8acb35c08630a030bae1f",
      "name": "Test_Haven"
    },
    "reportedBy": {
      "avatar": "https://api.havenapp.global/images/avatar.png",
      "_id": "5cc71f18a15f3c2bd836f66e",
      "username": "stephan"
    }
  },
  "status": 200
}
```

This endpoint retrieves a report by it's report_id, after verifying your Admin role.

### HTTP Request

`GET https://api.havenapp.global/v1/report/:report_id`

## Report a Haven by haven_id

```shell
curl -X POST \
  'https://api.havenapp.global/v1/report/haven/:haven_id' \
  -H 'Authorization: Bearer jsonwebtoken' \
  -d 'description=Your report message here'
```

> The above command returns JSON structured like this:

```json
{
  "success": "Haven report was succesfully generated",
  "report": {
    "_id": "5ce956999d66e749eee046a7",
    "type": "haven",
    "date": "2019-05-25T14:52:09.306Z",
    "havenId": "5cc8acb35c08630a030bae1f",
    "reportedBy": "5cc71f18a15f3c2bd836f66e",
    "createdAt": "2019-05-25T14:52:09.308Z",
    "updatedAt": "2019-05-25T14:52:09.308Z",
    "__v": 0
  },
  "status": 200
}
```

This endpoint generates a report for a Haven, based on the haven_id.

### HTTP Request

`POST https://api.havenapp.global/v1/report/haven/:haven_id`

### Query Parameters

| Parameter | Description                                        |
| --------- | -------------------------------------------------- |
| haven_id  | The id for the Haven you're sending a report about |

### Body Parameters

| Parameter   | Description                                                    |
| ----------- | -------------------------------------------------------------- |
| description | The report message specified by the person reporting the Haven |

## Report a User by user_id

```shell
curl -X POST \
  'https://api.havenapp.global/v1/report/user/:user_id' \
  -H 'Authorization: Bearer jsonwebtoken' \
  -d 'description=Your report message here'
```

> The above command returns JSON structured like this:

```json
{
  "success": "User report was succesfully generated",
  "report": {
    "_id": "5ce957c46135314d1474ee24",
    "type": "user",
    "description": "This Admin is wild, using a pretty wack avatar that does not fit this platform ey!",
    "date": "2019-05-25T14:57:08.501Z",
    "userId": "5cc719d7cae6c00faaebad34",
    "reportedBy": "5cc71f18a15f3c2bd836f66e",
    "createdAt": "2019-05-25T14:57:08.501Z",
    "updatedAt": "2019-05-25T14:57:08.501Z",
    "__v": 0
  },
  "status": 200
}
```

This endpoint generates a report for a User, based on the user_id.

### HTTP Request

`POST https://api.havenapp.global/v1/report/user/:user_id`

### Query Parameters

| Parameter | Description                                       |
| --------- | ------------------------------------------------- |
| user_id   | The id for the User you're sending a report about |

### Body Parameters

| Parameter   | Description                                                   |
| ----------- | ------------------------------------------------------------- |
| description | The report message specified by the person reporting the User |

## Report a Hangout by hangout_id

```shell
curl -X POST \
  'https://api.havenapp.global/v1/report/hangout/:hangout_id' \
  -H 'Authorization: Bearer jsonwebtoken' \
  -d 'description=Your report message here'
```

> The above command returns JSON structured like this:

```json
{
  "success": "Hangout report was succesfully generated",
  "report": {
    "_id": "5ce95a5965c5b353e97e78f2",
    "type": "hangout",
    "description": "This name is unacceptable, we can't mess with time.. this is outside of the scope of human influence!",
    "date": "2019-05-25T15:08:09.903Z",
    "hangoutId": "5cddce5345a54671917b7523",
    "reportedBy": "5cc71f18a15f3c2bd836f66e",
    "createdAt": "2019-05-25T15:08:09.904Z",
    "updatedAt": "2019-05-25T15:08:09.904Z",
    "__v": 0
  },
  "status": 200
}
```

This endpoint generates a report for a User, based on the user_id.

### HTTP Request

`POST https://api.havenapp.global/v1/report/hangout/:hangout_id`

### Query Parameters

| Parameter  | Description                                          |
| ---------- | ---------------------------------------------------- |
| hangout_id | The id for the Hangout you're sending a report about |

### Body Parameters

| Parameter   | Description                                                   |
| ----------- | ------------------------------------------------------------- |
| description | The report message specified by the person reporting the User |
