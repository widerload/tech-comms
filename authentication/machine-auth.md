# Machine Authentication

## Overview

It probably doesn't occur to people that machines need mechanisms to authenticate themselves, but its true. It is important that machines are able to provide their own authentication as part of broader trust relationships. For example when you log in at work your organisation doesn't want people signing in from just anywhere, they want people to sign in from devices that they control in an effort to protect themselves. So the machine needs to authenticate itself before the user, so that the user is able to authenticate themselves.

Moreover the distributed way many applications work now means that in reality when you do anything online there will be many layers of applications that are involved in fulfilling your request, and each of these will probably have its own authentication mechanisms.

## How does this work?

Surprisingly seemlessly, usually. There are really 3 kinds of technologies that machines use and its useful to start with something that you should already be familiar with, the username and password.

### Machine-name and Password

In this example we typically see the machine name substituted as the username, but actually when you realise that in this scenario we have no requirement for the username to be understood by a human so long as its understood that it can be 'translated' on both ends then some systems will use transformed, hardware generated, or pseudo-randomly generated data to perform the authentication and this information can also be held secret from the user, so a potential attacker has a hard time impersonating the machine in an organisation.

Similarly the password does not have to conform to the same kinds of readability requirements a human password does and is made of a very long pseudo-randomly generated array of bytes which is in turn kept very secret from users, so it can be known only to itself and the machine it has a login relationship with.

There are pro's and cons to this, it can be well trusted but it is difficult to change and it once its in place, meaning if there is a problem you probably need to give your machine to the support desk for a while. Using this in application development can also be slow when using it some ways, but quick when using it in others.

### Access Token (a.k.a. API Key)

This authentication method attempts to strike a balance between security and speed while also attempting to strike a balance between not being user-readable but still being user-shareable. Thats a tall order, but this method relies on a single very long string of numbers and letters which a machine can use for both  identification and authorisation. This is often used in web applications where it is common to see a practice known as 'stateless' communication and so every message needs a quick and simple way to prove its authenticity without establishing a 'stateful' connection.

### OAuth

This is a much improved suite of authentication mechanisms that are discussed elsewhere, but suffice to say this too supports machine to machine authentication and is becoming an increasingly popular choice for reputable organisations.
