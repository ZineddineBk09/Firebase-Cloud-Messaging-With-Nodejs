# Firebase Cloud Messaging With Nodejs
Hey copilot here is the node server code to document:
/**
 * @file This file contains the server code for sending push notifications using Firebase Cloud Messaging.
 */

import { initializeApp, applicationDefault } from 'firebase-admin/app'
import { getMessaging } from 'firebase-admin/messaging'
import express, { json } from 'express'
import cors from 'cors'
import dotenv from 'dotenv'

dotenv.config()

process.env.GOOGLE_APPLICATION_CREDENTIALS

const app = express()
app.use(express.json())

app.use(
  cors({
    origin: '*',
  })
)

app.use(
  cors({
    methods: ['GET', 'POST', 'DELETE', 'UPDATE', 'PUT', 'PATCH'],
  })
)

app.use(function (req, res, next) {
  res.setHeader('Content-Type', 'application/json')
  next()
})

/**
 * Initializes the Firebase app with the default credentials and project ID from the environment variables.
 */
initializeApp({
  credential: applicationDefault(),
  projectId: process.env.FIREBASE_PROJECT_ID,
})

/**
 * Endpoint for sending push notifications.
 * @name POST /send
 * @function
 * @param {Object} req - The request object.
 * @param {Object} req.body - The request body containing the title, body, and token for the push notification.
 * @param {string} req.body.title - The title of the push notification.
 * @param {string} req.body.body - The body of the push notification.
 * @param {string} req.body.token - The token of the device to receive the push notification.
 * @param {Object} res - The response object.
 */
app.post('/send', function (req, res) {
  const { title, body, token } = req.body

  const message = {
    notification: {
      title,
      body,
    },
    token,
  }

  getMessaging()
    .send(message)
    .then((response) => {
      res.status(200).json({
        message: 'Successfully sent message',
        token: token,
      })
      console.log('Successfully sent message:', response)
    })
    .catch((error) => {
      res.status(400)
      res.send(error)
      console.log('Error sending message:', error)
    })
})

app.listen(3000, function () {
  console.log('Server started on port 3000')
})

This is a simple example of how to send push notifications to Web, Android and iOS using Firebase Cloud Messaging (FCM) with Nodejs.

## Demo Video

![](./demo.mp4)

## Prerequisites

- Node.js
- Create a new project in the Firebase console
- Generate a new private key for the Firebase Admin SDK (firebase-credentials.json)
- Firebase Admin SDK
- Express
- Cors
- Dotenv

## Getting Started

1. Clone the repository:

```bash
git clone
```