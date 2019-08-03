# Outlook Calendar Bundle

This bundle use Outlook API for list events in Outlook Calendar.

Please feel free to contribute, to fork, to send merge request and to create ticket.

## Requirement
### Create an API account

Go to the application registration portal : https://apps.dev.microsoft.com

Click on "Add an app" and put a name to your app.

If you add platform native in settings go to Symfony 4 explain.

Click on "Generate New Password" and copy the password


## Installation
### Step 1: Install OutlookCalendarBundle

Run

```bash
composer require fungio/outlook-calendar-bundle:dev-master
```

### Step 2: Enable the bundle

``` php
<?php
// app/AppKernel.php

public function registerBundles()
{
    $bundles = [
        // ...
        new Fungio\OutlookCalendarBundle\FungioOutlookCalendarBundle()
    ];
}
```

### Step 3: Configuration

```yml
# app/config/parameters.yml

fungio_outlook_calendar:
    outlook_calendar:
        client_id: "YOUR_APPLICATION_ID"
        client_secret: "THE_PASSWORD_YOU_SAVED"
```

## Example

``` php
<?php
// in a controller
$request = $this->get('request_stack')->getMasterRequest();
$session = new Session();

$outlookCalendar = $this->get('fungio.outlook_calendar');
if ($session->has('fungio_outlook_calendar_access_token')) {
    // do nothing
} else if ($request->query->has('code') && $request->get('code')) {
    $token = $outlookCalendar->getTokenFromAuthCode($request->get('code'), $redirectUri);
    $access_token = $token['access_token'];
    $session->set('fungio_outlook_calendar_access_token', $access_token);
} else {
    return new RedirectResponse($outlookCalendar->getLoginUrl($redirectUri));
}

$events = $outlookCalendar->getEventsForDate($session->get('fungio_outlook_calendar_access_token'), new \DateTime('now');
```
# For Symfony 4 

Add var CLIENT_ID and CLIENT_SECRET to env config

Added new attribute to OutlookCalendar class native references to Microsoft App settings
platform added is native.

_
# Example Symfony 4
``` php
<?php
 $outlookClient= new \Fungio\OutlookCalendarBundle\Service\OutlookCalendar();
 $outlookClient->setNative();
 $token=$outlookClient->getTokenFromAuthCode($authcode, $redirect_uri);
``` 
 # Credits
 Create by Fungio fungio76@gmail.com
 
 Symfony 4 Fork jeyserEssedi jeyser@essedi.es
 
