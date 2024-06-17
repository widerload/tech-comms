# Trusted Device

## Overview

A trusted device is typically a kind of passkey but bound to a bespoke application, typically with some additional rules about how the authentication process works. Google for example will display a popup on your phone with a prompt to approve or deny, while microsoft's app additionally makes you select the correct number displayed on the login screen.

## Process Flow

```mermaid
flowchart TD
    start("Start")
    dtrust{"Trusted device /
    app linked?"}
    tdprompt1{"Use Trusted Device?"}
    nextoption("Next Option")
    tdprompt2("Follow instructions on trusted device")
    tdvalid{"Trusted device approved?"}
    reset("Reset password form")
    tddeclined{"Trusted Device Failed
    Try again or start over?"}
    endbox("End")

    start --> dtrust
    dtrust --"Yes"--> tdprompt1
    dtrust --"No"--> nextoption
    tdprompt1 --"Yes"--> tdprompt2 --> tdvalid
    tdprompt1 --"No"--> nextoption
    tdvalid --"Yes"--> reset
    tdvalid --"No"--> tddeclined
    tddeclined --"Try Again"--> tdprompt2
    tddeclined --"Start Over"--> start
    reset --> endbox
    nextoption --> endbox
```

### Description

If a trusted device is linked with the account and the user has prompted that they wish to use the trusted device for verification then typically a popup will appear on the device with further instructions. Note the device must be switched on an have internet access for this to work. Once approved the appropriate password reset page is displayed, however if this fails the user will have to either try again or try another option.
