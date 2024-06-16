# Process Variations

## Flow Chart

The overall flow chart for the forgotten password process looks like this. This is very complicated but all of these options are used by platforms large and small. Different platforms will use different options, but generally it looks something like this.

Additionally, some processes are preferred over others, and I have broadly tried to follow recommendations for what is considered most secure to least. Some of these processes may be unfamiliar to you and i have tried to summarise these below. For more details there will be a separate guide coming out at some point in future.

```mermaid
flowchart TD
    subgraph startend
    start("Start")
    uid[["Input username / email address"]]
    d1{"Server decides process
    based on what is supported 
    and available."}
    reset("Reset password form")
    recover("Account Recovery")
    endbox("End")

    start --"Click Forgotten Password Link"--> uid
    uid --"Click next"--> d1
    reset --> endbox
    recover --> endbox
    end

    d1 --> dfidoexists

    subgraph fido
    dfidoexists{"Is a FIDO2 Key on file?"}
    dfido2{"Is FIDO2 mandatory when it exists?"}
    fidoprompt1{"Validate FIDO2?"}
    fidoprompt2("Prompt user to plug in FIDO2 key")
    fido1("Plug in FIDO2 Key")
    fido2("Touch metal component")
    dfidovalid{"FIDO2 key valid?"}
    dfido22{"Is FIDO2 mandatory and exclusive?"}
    fidodeclined{"FIDO2
    Try again or start over?"}

    dfidoexists --"Yes"--> dfido2
    dfido2 --"Yes"--> fidoprompt2
    dfido2 --"No"--> fidoprompt1
    fidoprompt1 --"Yes"--> fidoprompt2
    fidoprompt2 --> fido1 --> fido2 --> dfidovalid
    dfidovalid --"No"--> dfido22
    dfido22 --"No"--> fidodeclined
    fidodeclined --"Try Again"--> fidoprompt2
    end

    dfidoexists --"No"--> dpasskey
    fidoprompt1 --"No"--> dpasskey
    dfidovalid --"Yes"--> reset
    dfido22 --"Yes"--> endbox
    fidodeclined --"Start Over"--> start

    subgraph passkey
    dpasskey{"Passkey on file?"}
    pkprompt1{"Validate Passkey?"}
    pkprompt2("Connect Passkey provider")
    dpkvalid{"Passkey valid?"}
    pkdeclined{"Passkey
    Try again or start over?"}

    dpasskey --"Yes"--> pkprompt1
    pkprompt1 --"Yes"--> pkprompt2
    pkprompt2 --"Success"--> dpkvalid
    pkprompt2 --"Failed"--> pkdeclined
    dpkvalid --"No"--> pkdeclined
    pkdeclined --"Try Again"--> pkprompt2
    end

    dpasskey --"No"--> dtrust
    pkprompt1 --"No"--> dtrust
    dpkvalid --"Yes"--> reset
    pkdeclined --"Start Over"--> start

    subgraph trustdevice
    dtrust{"Trusted device /
    app linked?"}
    tdprompt1{"Use Trusted Device?"}
    tdprompt2("Follow instructions on trusted device")
    tdvalid{"Trusted device approved?"}
    tddeclined{"Trusted Device
    Try again or start over?"}

    dtrust --"Yes"--> tdprompt1
    tdprompt1 --"Yes"--> tdprompt2 --> tdvalid
    tdvalid --"No"--> tddeclined
    tddeclined --"Try Again"--> tdprompt2
    end

    dtrust --"No"--> dotp
    tdvalid --"Yes"--> reset
    tddeclined --"Start Over"--> start

    subgraph otp
    dotp{"OTP Key on file?"}
    otpprompt{"Use code from 
    app / device?"}
    otp1[["Enter code"]]
    otpvalid{"OTP code valid?"}
    otpdeclined{"OTP
    Try again or start over?"}

    dotp --"Yes"--> otpprompt
    otpprompt --"Yes"--> otp1 --> otpvalid
    otpvalid --"No"--> otpdeclined
    otpdeclined --"Try Again"--> otp1
    end

    dotp --"No"--> demail
    otpprompt --"No"--> demail
    otpvalid --"Yes"--> reset
    otpdeclined --"Start Over"--> start

    subgraph email
    demail{"email / backup 
    email on file?"}
    demail2{"Support code, or link, or both?"}
    emcodeprompt{"Send a code to my email?"}
    emcode1("Emails code to email")
    emcode2[["Input code"]]
    emcodevalid{"email code valid?"}
    emlinkprompt{"Send a link to my email?"}
    emlink1("Emails link to email")
    emlink2("User clicks link")
    emlinkactive{"Link still active?"}
    embothprompt{"Send a code or a link to my email?"}
    emdeclined{"email
    Try again or start over?"}

    demail --"Yes"--> demail2
    demail2 --"Code"--> emcodeprompt
    emcodeprompt --"Yes"--> emcode1 --> emcode2 --> emcodevalid
    emcodevalid --"No"--> emdeclined
    demail2 --"Link"--> emlinkprompt
    emlinkprompt --"Yes"--> emlink1 --> emlink2 --> emlinkactive
    emlinkactive --"No"--> emdeclined
    demail2 --"Both"--> embothprompt
    embothprompt --"Code"--> emcode1
    embothprompt --"Link"--> emlink1
    emdeclined --"Try Again"--> demail2
    end

    demail --"No"--> dphone
    emcodeprompt --"No"--> dphone
    emcodevalid --"Yes"--> reset
    emlinkprompt --"No"--> dphone
    emlinkactive --"Yes"--> reset
    embothprompt --"Neither"--> dphone
    emdeclined --"Start Over"--> start

    subgraph phone    
    dphone{"Phone number on file?"}
    dphone2{"Support SMS or Call?"}
    dphonesmsprompt{"Send code to my phone?"}
    phonesms1("Texts code to phone")
    dphonecallprompt{"Call my phone?"}
    phonecall1("Calls phone")
    phonebothprompt{"Send sms message or call?"}
    phone2[["Input code"]]
    phonevalid{"Code valid?"}
    phonedeclined{"phone
    Try again or start over?"}

    dphone --"Yes"--> dphone2
    dphone2 --"SMS"--> dphonesmsprompt
    dphone2 --"Call"--> dphonecallprompt
    dphone2 --"Both"--> phonebothprompt
    dphonesmsprompt --"Yes"--> phonesms1 --> phone2 --> phonevalid
    dphonecallprompt --"Yes"--> phonecall1
    phonebothprompt --"SMS"--> phonesms1
    phonebothprompt --"Call"--> phonecall1
    phonecall1 --"Answered"--> phone2
    phonecall1 --"Timeout"--> phonedeclined
    phonevalid --"No"--> phonedeclined
    phonedeclined --"Try Again"--> dphone2
    end

    dphone --"No"--> dbkpcode
    dphonesmsprompt --"No"--> dbkpcode
    dphonecallprompt --"No"--> dbkpcode
    phonebothprompt --"Neither"--> dbkpcode
    phonevalid --"Yes"--> reset
    phonedeclined --"Start Over"--> start

    subgraph backupcodes
    dbkpcode{"Are there backup codes on file?"}
    bkpcodeprompt{"Do you want to use a backup code?"}
    bkpcode1[["Input backup code"]]
    bkpcodevalid1{"Is the code valid?"}
    bkpcodevalid2{"Has the code been used before?"}
    bkpcodedeclined{"Backup Code
    Try again or start over?"}

    dbkpcode --"Yes"--> bkpcodeprompt
    bkpcodeprompt --"Yes"--> bkpcode1 --> bkpcodevalid1
    bkpcodevalid1 --"Yes"--> bkpcodevalid2
    bkpcodevalid1 --"No"--> bkpcodedeclined
    bkpcodevalid2 --"Yes"--> bkpcodedeclined
    bkpcodedeclined --"Try Again"--> bkpcode1
    end

    dbkpcode --"No"--> ddelg
    bkpcodeprompt --"No"--> ddelg
    bkpcodevalid2 --"No"--> reset
    bkpcodedeclined --"Start Over"--> start

    subgraph delegate
    ddelg{"Delegate 
    (spouse / parent / assistant) 
    on file"}
    delgprompt{"Send prompt to delegate?"}
    delg1("Sends prompt to delegate")
    delg2("Delegate resets password")

    ddelg --"Yes"--> delgprompt
    delgprompt --"Yes"--> delg1 --> delg2
    end

    ddelg --"No"--> dsq
    delgprompt --"No"--> dsq
    delg2 --> endbox

    subgraph secretq
    dsq{"Secret questions 
    on file?"}
    sqprompt{"Answer secret questions?"}
    sq1("Select random questions")
    sq2[["Answer questions"]]
    sqvalid{"All answers correct"}
    sqdeclined{"Secret Questions
    Try again or start over?"}

    dsq --"Yes"--> sqprompt
    sqprompt --"Yes"--> sq1 --> sq2 --> sqvalid
    sqvalid --"No"--> sqdeclined
    sqdeclined --"Try Again"--> sq1
    end

    dsq --"No"--> doldpw
    sqprompt --"No"--> doldpw
    sqvalid --"Yes"--> reset
    sqdeclined --"Start Over"--> start
    
    subgraph oldpw
    doldpw{"Are there old 
    passwords on file?"}
    oldpwprompt("Do you remember 
    an old password?")
    oldpw1[["Input old password"]]
    oldpwvalid{"Does it match any 
    recent passwords?"}
    oldpwdeclined{"Old password
    Try again or start over?"}

    doldpw --"Yes"--> oldpwprompt
    oldpwprompt --"Yes"--> oldpw1 --> oldpwvalid
    oldpwvalid --"No"--> oldpwdeclined
    oldpwdeclined --"Try Again"--> oldpw1
    end

    doldpw --"No"--> dnone
    oldpwprompt --"No"--> dnone
    oldpwvalid --"Yes"--> reset
    oldpwdeclined --"Start Over"--> start

    subgraph none
    dnone("None available")    
    end

    dnone --> recover
```

## Further Details

Further Details for each section of the above can be found below.

1. [Yubikey / FIDO2](FIDO2.md)

