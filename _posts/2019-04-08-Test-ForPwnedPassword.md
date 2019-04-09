---
title: "Test-ForPwnedPassword"
excerpt: "Function to check for a password that is available in the compromised password dataset over at https://api.pwnedpasswords.com."
date: 2019-04-08
last_modified_at: 2019-04-08
comments: true
tags:
  - PowerShell
  - Security
  - Passwords
  - Function
---

Test-ForPwnedPassword is a function to check the API hosted at api.pwnedpasswords.com for passwords that have been found in a data breach.

## usage

### example 1 - SecureString Password

Tests the submitted password (converted to the SHA1 hash value) to see if it is present in the API's data set as a breached password.

``` powershell
$Password = Read-Host -AsSecureString
Test-ForPwnedPassword -Password $Password
```

#### Output

This example will produce a boolean output. True if the password exists in the APIs breached password database, false if it does not.

### example 2 - Hash with

Tests the password hash for the password 'Micro$oft' to see if it is present in the API's data set as a breached password and specifies the IncludeInstanceCount parameter.

``` powershell
Test-ForPwnedPassword -Hash 9cd277f71f1a9d77eccb441836bdea6f1b5c2685 -IncludeInstanceCount
```

#### Output

This example will produce a PSCustomObject output with attributes FoundInPwnedPasswordsAPI and InstanceCount as shown below:

![no-alignment]({{ site.url }}{{ site.baseurl }}/assets/images/TestForPwnedPasswordObjectOutput.png)

### example 3 - Password Prompting

Prompts for a password and tests the password hash to see if it is present in the API's data set as a breached password.

Don't use this syntax if you are remoting to a non-windows machine due to [problems with SecureString being passed between sessions](https://github.com/PowerShell/PowerShell/issues/7239).
{: .notice--warning}

``` powershell
Test-ForPwnedPassword
```

### using the pipeline

You can pipe one or more securestring or string objects into Test-ForPwnedPassword.  Strings will bind to the Hash parameter and SecureStrings will bind to the Password parameter.

## background

I had previously been aware of and used the email address lookup tool over at [Troy Hunt's haveibeenpwned](https://haveibeenpwned.com/) site, but I had not noticed the compromised (pwned) passwords lookup that was also available there. That all changed while I was listening to a recent episode of [Microsoft Cloud Show](http://www.microsoftcloudshow.com/podcast/Episodes/296-have-i-been-pwned-an-interview-with-troy-hunt). I highly recommend this podcast if you are into Microsoft Cloud technologies.  I found out that [Troy](https://twitter.com/troyhunt) released a v3 of the pwned passwords API [in July 2018](https://www.troyhunt.com/pwned-passwords-v3-is-now-live/) so I went looking for any existing PowerShell function to use for automating these lookups. That search yielded the [Get-PwnedPassword](https://sqldbawithabeard.com/2017/08/09/using-powershell-to-check-if-your-password-has-been-in-a-breach/) function from [SQLDBAwithTheBeard](https://robsewell.info/) which uses one of the older APIs. The v3 API documentation is [here](https://haveibeenpwned.com/API/v2#PwnedPasswords).

## function details

I started from the Get-PwnedPassword function and modified it to use the v3 API and to use the Test verb and by default to just return True if the password submitted is found in the API records and False if it is not found.  Additionally, I added a parameter IncludedInstanceCount to return an object with the following attributes:

- FoundInPwnedPasswordsAPI: returns a boolean value, true if found, false if not
- InstanceCount: returns the count that is returned by the API for how many times the password is found in the API dataset.

I also preserved the Hash parameter (and parameter set) from Rob Sewell's original Get-PwnedPassword function so that you can, if you wish, submit your own hashed values to avoid having Test-ForPwnedPassword have access to the actual password value. If you do use the Password parameter (and parameter set) the password must be submitted as a secure string (or you will be prompted for a secure string if you use the function interactively without providing a value to the Password parameter) and the password is converted to plain text in memore to be hashed by private function Get-StringHash (code for Get-StringHash is in the Begin block of the function). In either case, only the first 5 characters of the hash are sent to the remote API using Invoke-RestMethod.

After writing this function and while preparing this blog post, I also found a [Test-PwnedPassword function by Fredrik Kacsmarck](http://psfredrik.chiloma.com/author/frekac/). Fredrik and I took very similar approaches to this functionality with the main difference being Test-ForPwnedPassword just returns a boolean value by default and Test-ForPwnedPassword allowing for you to produce/provide your own hashes in some way of your choosing.

## PowerShellGallery

It's available [here](https://www.powershellgallery.com/packages/Test-ForPwnedPassword/0.1.0) in the PowerShell gallery and can be installed by running the following in a PowerShell session that has the PowerShellGet Module installed:

```Powershell
Install-Script -Name Test-ForPwnedPassword
```

## feedback

Any improvements to this function that you might recommend? I'm considering developing a few other functions to take advantage of the other APIs available at haveibeenpwned.com.  Would you have a use for that?

## code

[Source Code](https://github.com/exactmike/Profile/blob/c5e2c775f4eba8a659521bb06069154dc35b086d/functions/Test-ForPwnedPassword.ps1#L45) is currently hosted in my profile module which I use for personal powershell functions that I load with my powershell profile.  Contributions are welcome!
