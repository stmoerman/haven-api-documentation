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
  -d 'email=your@email.com&password=password'
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
  "message": "Registration successful."
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

# User

## Retrieve current user

```shell
curl -X GET \
  'https://api.havenapp.global/v1/users/current' \
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
  'https://api.havenapp.global/v1/users/:user_id' \
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

| Parameter | Description                                                     |
| --------- | --------------------------------------------------------------- |
| user_id   | The id for the User you're trying to retrieve information about |

## Update your user account details

```shell
curl -X PUT \
  'https://api.havenapp.global/v1/users/:user_id' \
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
  -H 'Authorization: Bearer jsonwetoken'
```

> The above command returns JSON structured like this:

```json
{
  "havens": [
    {
      "_id": "5cb611ded015660d4183722c",
      "name": "C++",
      "tagline": "Software for world changers",
      "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1555435998/haven/havens/banners/d9xq1udw9508wzlwffrs.jpg"
    },
    {
      "_id": "5cb6123dd015660d4183722d",
      "name": "Apple",
      "tagline": "Scamming the world, one repair at a time",
      "banner": "https://res.cloudinary.com/scope-web-llc/image/upload/v1555584522/sl2gvx6c9prxl5ijxwp1.png"
    }
  ]
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
  -H 'Authorization: Bearer jsonwetoken'
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "msg": "Your account was succesfully deleted."
}
```

This endpoint deletes the user account for the provided user_id param, as long as it matches the user_id of the requester. All data belonging to the user is automatically purged and deleted.

### HTTP Request

`PUT https://api.havenapp.global/v1/users/:user_id`

### Query Parameters

| Parameter | Description               |
| --------- | ------------------------- |
| user_id   | Your user account user_id |

# Haven

## Create a new Haven

```shell
curl -X POST \
  'https://api.havenapp.global/v1/haven/' \
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

| Parameter | Description                                                         |
| --------- | ------------------------------------------------------------------- |
| haven_id  | The id for the Haven you want to delete (as the owner of the Haven) |

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

| Parameter | Description                                                    |
| --------- | -------------------------------------------------------------- |
| haven_id  | The id for the Haven you want to retrieve the information from |

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

| Parameter | Description                                                    |
| --------- | -------------------------------------------------------------- |
| haven_id  | The id for the Haven you want to retrieve the information from |

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

| Parameter | Description                           |
| --------- | ------------------------------------- |
| haven_id  | The id for the Haven you want to join |

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

| Parameter | Description                                                    |
| --------- | -------------------------------------------------------------- |
| haven_id  | The id for the Haven you want to retrieve the information from |

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
  -H 'Authorization: Bearer jsonwetoken' \
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

# Hangout

## Create a new post

```shell
curl -X POST \
  'https://api.havenapp.global/v1/hangout/:haven_id/post' \
  -H 'Authorization: Bearer jsonwetoken' \
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
  -H 'Authorization: Bearer jsonwetoken' \
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
  -H 'Authorization: Bearer jsonwetoken' \
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
  -H 'Authorization: Bearer jsonwetoken' \
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
}Post an announcement (as owner or admin)
```

This endpoint allows you to delete a Hangout & the conversation belonging to it.

### HTTP Request

`DELETE https://api.havenapp.global/v1/hangout/:hangout_id`

### Query Parameters

| Parameter  | Description                               |
| ---------- | ----------------------------------------- |
| hangout_id | The id for the Hangout you want to delete |

# Chat

## Start a new private chat

```shell
curl -X POST \
  'https://api.havenapp.global/v1/chat/new/:user_id' \
  -H 'Authorization: Bearer jsonwetoken' \
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
  -H 'Authorization: Bearer jsonwetoken' \
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
  -H 'Authorization: Bearer jsonwetoken' \
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
  -H 'Authorization: Bearer jsonwetoken'
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
  -H 'Authorization: Bearer jsonwetoken' \
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
