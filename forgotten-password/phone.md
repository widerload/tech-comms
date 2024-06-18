# Phone

## Overview

The option to use your phone is also an extension of the OTP mechanism. Users can receive a code either by SMS, some other messaging application (e.g. WhatsApp) or via an automated call. Much like with email users have an amount of time to submit that code into the prompt on a website for validation.

This is typically considered a medium security option (or in some countries low) because there are ways an attacker can clone or deny you access your phone number in favour of them. So when using this security model this is a risk to consider. Fortunately in many highly developed nations security around this type of attack has improved over the last 30 years and these days instances of this type of attack are uncommon, but they do still happen.

## Process Flow

```mermaid
flowchart TD
    start("Start")
    dphone{"Phone number on file?"}
    dphone2{"Support SMS or Call?"}
    nextoption("Next Option")
    dphonesmsprompt{"Send code to my phone?"}
    dphonecallprompt{"Call my phone?"}
    phonebothprompt{"Send sms message or call?"}
    phonesms1("Texts code to phone")
    phonecall1("Calls phone")
    inputcode[["Input code"]]
    phonevalid{"Code valid?"}
    reset("Reset password form")
    phonedeclined{"phone call or SMS 
    invalid or did not answer
    Try again or start over?"}
    endbox("End")

    start --> dphone
    dphone --"Yes"--> dphone2
    dphone --"No"--> nextoption
    dphone2 --"SMS"--> dphonesmsprompt
    dphone2 --"Call"--> dphonecallprompt
    dphone2 --"Both"--> phonebothprompt
    dphone2 --"Neither"--> nextoption
    dphonesmsprompt --"Yes"--> phonesms1
    dphonecallprompt --"Yes"--> phonecall1
    phonebothprompt --"SMS"--> phonesms1
    phonebothprompt --"Call"--> phonecall1
    dphonesmsprompt --"No"--> nextoption
    dphonecallprompt --"No"--> nextoption
    phonebothprompt --"Neither"--> nextoption
    nextoption --> endbox
    phonesms1 --> inputcode
    phonecall1 --"Answered"--> inputcode
    phonecall1 --"Missed Call"--> phonedeclined
    inputcode --> phonevalid
    phonevalid --"Yes"--> reset
    phonevalid --"No"--> phonedeclined
    phonedeclined --"Try Again"--> dphone2
    phonedeclined --"Start Over"--> start
    reset --> endbox
```

### Description

In summary if we have a number on file and the system supports sending codes to phone numbers then it gives the user an option based on what it supports - SMS, Call, or either. If the user chooses one of these options then the code is sent to them and they must supply it to the system they are on. If the code is valid they can reset their password, if not, or if it has timed out, or they failed to answer the call, then they must try again.
