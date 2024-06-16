# Fast Identity Online (FIDO2)

Also known by some companies as Yubikey, or Google Titan. But these are mostly just branding for two popular implementations of the standard.

This is a process that I use in many places, including Google and I specifically use a number of Yubikey devices as part of this practice.

## Overview

This is generally considered the most secure option, but it has some drawbacks which mean it is not suitable for widespread adoption. However, this is commonly used in extremely high security scenarios where it is better that nobody has access to your account than the the wrong person. It is commonly used in governments, military / national security agencies, big technology companies, or by public figures, journalists, whistleblowers, dissidents, and many others including enthusiasts (nutters) like me. If lives could be at risk if someone breaks into your stuff, use this.

### Risks

The drawbacks however are that some organisations will configure their systems so that if a FIDO2 key exists then it is the only mecahnism that can unlock your account. Meaning, if you lose your keys, there is nothing anybody can do. Additionally these are physical devices which cost money and it is recommended you buy a minimum of two in case you lose one. After that its down to your operational security practices to ensure you dont lose access to one and nobody else can get access to your backup keys, and if somebody does get access to a key you need to remove its access as a priority.


## Process Flow

A breakdown of the FIDO2 process flow looks like this:

```mermaid
flowchart TD
    start("Start")
    dfidoexists{"Is there a FIDO2 
    Key on file?"}
    dfido2{"Is FIDO2 mandatory 
    when it exists?"}
    fidoprompt1{"Ask user if they 
    want to use FIDO2?"}
    fidoprompt2("Prompt user to plug in FIDO2 device")
    fido1[["Plug in FIDO2 device"]]
    fido2[["User Touches metal component
    or fingerprint scanner"]]
    dfidovalid{"FIDO2 key found
    and valid?"}
    dfido22{"Is FIDO2 mandatory
     and exclusive?"}
    fidodeclined{"Try again?"}
    nextoption("Next Option")
    reset("Show password reset form")
    endbox("End")

    start --> dfidoexists
    dfidoexists --"Yes"--> dfido2
    dfidoexists --"No"--> nextoption
    dfido2 --"Yes"--> fidoprompt2
    dfido2 --"No"--> fidoprompt1
    fidoprompt1 --"Click Yes"--> fidoprompt2
    fidoprompt1 --"Click No"--> nextoption
    fidoprompt2 --> fido1 --> fido2 --> dfidovalid
    dfidovalid --"Yes"--> reset
    dfidovalid --"No"--> dfido22
    dfido22 --"Yes"--> endbox
    dfido22 --"No"--> fidodeclined
    fidodeclined --"Yes"--> fidoprompt2
    fidodeclined --"No"--> endbox
    reset --> endbox
```

### Description

If the system has a FIDO2 key on file for you then it checks its setup to see if the FIDO2 key is mandatory to login. If so then it will prompt you to plug in the FIDO2 device, otherwise it will ask you if you want to use FIDO2 for authentication. Assuming you do, you plug in the device and typically either touch a small metal componend or put the appropriate finger on the fingerprint sensor. The system checks your fingerprint and looks for the appropriate key and if it is found the system will negotiate with the FIDO2 device to validate a secret known only to the system and the FIDO2 device. If this passes then everything is good, if not then you will probably get the option to try again with a backup device, but failing that there is nothing that can be done.
