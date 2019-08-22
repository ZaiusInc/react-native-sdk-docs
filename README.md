# Getting Started

## Overview

The Zaius React Native SDK module is an NPM package that will allow you to easily interact with the Zaius API via a "Mobile Apps" integration with an app that is written in React Native.

You can get started developing with Zaius with the [Zaius Documentation](https://developer.zaius.com/core-concepts/). This guide will document how to use the Zaius React Native SDK to interact with the Zaius API.

## Create Mobile Apps in Zaius UI

In order to use Zaius in your React Native application, you will first need to set up a Mobile App integration on the Zaius web app. In your Zaius account:

1. Click on "Account Settings" in the top-right  
2. Click on "Integrations" in the left-hand sidebar  
3. Click on the "Mobile Apps" card in the list  
4. Create a new mobile app, name it, and remember the "short identifier". This will be used as the "app\_id" later.  
5. \(Optional\) install any necessary APNs keys or GCM keys that you need for push notifications. **Note:** Push notifications will not work unless you install keys here.

Your React Native app is now ready to begin integration with the SDK module.

