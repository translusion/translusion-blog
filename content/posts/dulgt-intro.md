---
title: "Deterministic Password Generator"
description: "An alternative to password keepers such as 1Password."
date: "2014-05-04"
categories: 
    - "Cryptography"
    - "Passwords"
    - "Login"
    - "Project"
---

The heartbleed exploit made  me thing of my own way of keeping passwords which relies on 1Password. What was painfull with 1Password was having to regenerate every single password I have over again. That is a lot of work. The second problem because all my passwords are too hard to remember, I am screwed if I ever lose my encrypted database of passwords. So I also make paper copies, but they have to be made secure as well, so they can not be used directly in their paper form.

So this has left me with desirng a better solution. Deterministic password generators seem attractive, but there is no one program that works exactly how I want it to work. But there are a of neat ideas out there. 

# Existing Solutions
### Enigma

[Enigma][enigma] looks pretty cool, but they had horrible customer support and have trouble with basic english. So not the kind of people I want to entrust with generating my passwords. Should I get a problem with their solution I need crystal clear communication. Also any solution should be open source, so you know exactly how the passwords are made and which makes it possible for you to roll your own solution should the company go belly up.

### Generator without Single Point of failure

John Metta has some interesting [ideas][singlepoint] on how to do this, but he doesn't have a product yet that he can vouch for. Anyway I'll keep my eye on his progress.

### Pronouncable Passwords
 
I kind of like pronounable passwords because they are far easier to remember. [PassKool][passkool] is a python script which uses this approach. However I have to check how safe this is. Making them pronounable of course reduces the search space for a hacker. [Passphrase FAQ][passphrasefaq] might be the place to learn how to do it.

# What I want in a Solution

1. Open source
2. Based on well established standards which are can be verified to be safe without being a crypto expert.
3. Should be very easy to recreate the code for generating the passwords, should the original program get lost.
4. On multiple platforms. I want the tool accessible from my phone, desktop, browser and command line.
5. Stores a list of sites or places I have made passwords for and settings used, so I don't forget that I actually have a password already for said site. I actually often forget that.
6. Should be easy to use. So I should mostly have to write a really easy password.
7. Paper backups. Should be easy to print out a piece of paper from the app containing key information. E.g. by using QR codes so it is really easy to restore a backup.
8. Should be secure, so that hackers can't easily break my master password once they get hold of one of my passwords.

(6) and (8) are obviously at odds with each other. My approach here would be to rely on certain assumptions:

* An attacker will most likely attack me online.
* He will not break into my house.

So my current idea for a solution relies on the following:

1. One simple password whic his what is mostly asked of me.
2. A hard to guess completely random component which I can not hope to remember. This will be stored on paper and will remain in memory for a limited amount of time. So occasionally it will need to be brought back using a QR code scanner e.g. Or if no camera is present with manual typing.
3. The on paper is encrypted with my simple password so that should somebody accidentally get hold of my paper backup then can't use it unless they know something about hacking.

The security of the individual points can be tweaked. The assumption here is that ultimately nothing is completely secure. It is all about making it really inconveient for the hacker. If you don't have important secrets or are not an important or wealthy person, we can assume that less security is needed, so that people may e.g. chose themselves whether the paper data should be encrypted and e.g. for how long the hard passwords should remain valid in memory. If your security needs are lower you could let the password stay indefintitly in memory. You could even store it permanently on disk.

There should be a lot of options to make sure the generator works for everyones needs.

## Technologies I should probably use

I should probably use [HMAC](http://en.wikipedia.org/wiki/Hash-based_message_authentication_code)(Hash-based message authentication code) because that is designed to hash using a secret key. Normally we hash like this:

	digest = hash(message, salt)
	
With HMAC it is:

	digest = hash(message, salt, secret)
	
This is better than doing:

	digest = hash(message || secret, salt)
	
where `||` means concatenate. Apparently this is potentially exploitable because you can create the hash without knowing all of the separate parts.

But I don't think HMAC is specifically design for generating passwords. So using `bcrypt` or `scrypt` might be better. Potentially I could combine them with HMAC in some way.

[HMAC in standard Go package](http://golang.org/pkg/crypto/hmac/) Takes as argument a hash function, key and message.<br>

[scrypt for Go](https://code.google.com/p/go/source/browse/scrypt/scrypt.go?repo=crypto) It uses a password, salt, some parameter which specify CPU and memory usage to slow it down. We could perhaps input generated key from scrypt into HMAC<br>

[All crypto functions you might need in Go](https://code.google.com/p/go/source/browse/?repo=crypto) This contains bcrypt, blowfish, SHA3, PBKDF2 etc.<br>

# List of existing solutions
[xanthir.com](http://www.xanthir.com/password/) Configure length, allowed characters. Poor security with SHA-1 hash<br>

[plevyak.com](http://plevyak.com/dpg.html) Skein-512 based. Looks promising with use of option distinguishing Information<br>

[SuperGenPass](http://supergenpass.com) Made to drop down from bookmarks and work on mobile homescreen. Easy to use but doesn't seem to be based on generally accepted good practice. In source code it looks like MD5 or SHA1 is used with just 10 iterators. That seems like bad choices. Should be far more iterations and with a better hash function such as SHA2 or SHA3 or bcrypt.<br>

[Standford PwdHash](https://www.pwdhash.com) Transparently converts a user password to a domain specific password. Is possibly using MD5 for hashing which is not desirably. Creator also reserves the right to change the tool.

[Fuktommy genpasswd](http://fuktommy.com/genpasswd/) A very simple version using SHA-1. Easy to read code. No iterations (no stretching) so poor security.<br>

[PassHash](http://passhash.connorhd.co.uk) Very simple. Uses SHA246-HMAC, but likely no iterations.<br>

[Secure Passwords](http://ctrlq.org/passwords/) Clean design using bcrypt. A bit too simple perhaps, but with good help instructions.<br>
# Resources

[crackstation](https://crackstation.net/hashing-security.htm) Great info on hashing. E.g. on why you shouldn't invent your own wacky hashing solutions by combining existig ones. Correct use of salt etc.<br>

[enigma]: https://itunes.apple.com/us/app/enigma/id527555438?mt=12
[singlepoint]: http://mettadore.com/ruby/secure-password-generator-as-manager-without-single-point-failure/
[passkool]: http://passkool.sourceforge.net
[passphrasefaq]: http://www.iusmentis.com/security/passphrasefaq/