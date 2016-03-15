PHP-PayPal-IPN
==============

Forked from: https://github.com/WadeShuler/PHP-PayPal-IPN

Forked from: https://github.com/Quixotix/PHP-PayPal-IPN/

Forked from the great Quixotix PayPal IPN script, which is no longer maintained. From now on, you should use this repo instead, as I have adddressed it's issues and brought it back to life.

This fork fixes the known issues with the original repo, as well as updates the code according to PayPal's documentation, and today's standards.

**NOTICE: The SSLv3 issue is fixed!**

**This has been fixed and works out of the box, the old `Quixotix` repo no longer works!**

**Requires:** PHP >= 5.3

A PayPal Instant Payment Notification (IPN) class for PHP >= 5.3 (if you aren't on at least 5.3, then I can't help you! I will not support dead versions!)

Use the `IpnListener` class in your PHP IPN script to handle the encoding of POST data, post back to PayPal, and parsing of the response from PayPal.

Install with Composer
---------------------

Composer is now supported!

Packagist: https://packagist.org/packages/wadeshuler/php-paypal-ipn

composer.json

    {
        "require": {
            "wadeshuler/php-paypal-ipn": "*"
        }
    }


    use wadeshuler\paypalipn\IpnListener;
    $listener = new IpnListener();

    // default options
    $listener->use_sandbox = true;
    $listener->use_curl = true;
    $listener->follow_location = false;
    $listener->timeout = 30; 
    $listener->verify_ssl = true;
    $listener->curl_ssl = false_; //Curl specify using SSL 6 (TLS 1.2)

    if ($verified = $listener->processIpn())
    {
        // handle successful ipn request
    } else {
        // handle invalid ipn request
    }

Please see the example file in `example\ipn.php`

Features
--------

* Switch between live and sandbox by setting the `use_sandbox` property.
* Supports both secure SSL and plain HTTP transactions by setting the `use_ssl`
  property (SSL is recommended).
* Supports both cURL and fsockopen network libraries by setting the `use_curl`
  property (cURL is recommended).
* Verifies an HTTP &quot;200&quot; response status code from the PayPal server.
* Get detailed plain text reports of the entire IPN using the `getTextReport()`
  method for use in emails and logs to administrators.
* Throws various exceptions to differentiate between common errors in code or
  server configuration versus invalid IPN responses.


Getting Started
---------------

This code is intended for web developers. You should understand how the IPN
process works conceptually and you should understand when and why you would be
using IPN. Reading the [PayPal Instant Payment Notification Guide][1] is a good
place to start.

You should also have a [PayPal Sandbox Account][2] with a test buyer account and
a test seller account. When logged into your sandbox account there is an IPN
simulator under the 'Test Tools' menu which you can used to test your IPN
listener.

[1]: https://cms.paypal.com/cms_content/US/en_US/files/developer/IPNGuide.pdf
[2]: https://developer.paypal.com

Once you have your sandbox account setup, you simply create a PHP script that
will be your IPN listener. In that script, use the `IpnListener()` class as shown
below. For a more thoroughly documented example, take a look at the
`example/ipn.php` script in the source code.

    <?php

    require_once('vendor/autoload.php');

    $listener = new \wadeshuler\paypalipn\IpnListener();
    $listener->use_sandbox = true;

    if ($verified = $listener->processIpn())
    {
        // Valid IPN
        /*
            1. Check that $_POST['payment_status'] is "Completed"
            2. Check that $_POST['txn_id'] has not been previously processed
            3. Check that $_POST['receiver_email'] is your Primary PayPal email
            4. Check that $_POST['payment_amount'] and $_POST['payment_currency'] are correct
        */

    } else {

        // Invalid IPN

    }

    ?>


Documentation
-------------

Documentation has not been generated yet, but, there are phpDocumentor style
docstrings (comments) throughout `IpnListener.php` which explain the important public properties and methods.

I have also written a more in-depth IPN tutorial on my blog: [PayPal IPN with PHP][3]

[3]: http://www.micahcarrick.com/paypal-ipn-with-php.html


Example Report
--------------

Here is an example of a report returned by the `getTextReport()` method. Create
your own reports by extending the `IpnListener()` class or by accessing the data
directly in your ipn script.

    --------------------------------------------------------------------------------
    [09/09/2011 8:35 AM] - https://www.sandbox.paypal.com/cgi-bin/webscr (curl)
    --------------------------------------------------------------------------------
    HTTP/1.1 200 OK
    Date: Fri, 09 Sep 2011 13:35:39 GMT
    Server: Apache
    X-Frame-Options: SAMEORIGIN
    Set-Cookie: c9MWDuvPtT9GIMyPc3jwol1VSlO=Ch-NORlHUjlmbEm__KG9LupR4mfMfQTkx1QQ6hHDyc0RImWr88NY_ILeICENiwtVX3iw4jEnT1-1gccYjQafWrQCkDmiykNT8TeDUg7R7L0D9bQm47PTG8MafmrpyrUAxQfst0%7c_jG1ZL6CffJgwrC-stQeqni04tKaYSIZqyqhFU7tKnV520wiYOw0hwk5Ehrh3hLDvBxkpm%7cYTFdl0w0YpEqxu0D1jDTVTlEGXlmLs4wob2Glu9htpZkFV9O2aCyfQ4CvA2kLJmlI6YiXm%7c1315575340; domain=.paypal.com; path=/; Secure; HttpOnly
    Set-Cookie: cookie_check=yes; expires=Mon, 06-Sep-2021 13:35:40 GMT; domain=.paypal.com; path=/; Secure; HttpOnly
    Set-Cookie: navcmd=_notify-validate; domain=.paypal.com; path=/; Secure; HttpOnly
    Set-Cookie: navlns=0.0; expires=Thu, 04-Sep-2031 13:35:40 GMT; domain=.paypal.com; path=/; Secure; HttpOnly
    Set-Cookie: Apache=10.72.109.11.1315575339707456; path=/; expires=Sun, 01-Sep-41 13:35:39 GMT
    X-Cnection: close
    Transfer-Encoding: chunked
    Content-Type: text/html; charset=UTF-8

    VERIFIED
    --------------------------------------------------------------------------------
    test_ipn                 1
    payment_type             instant
    payment_date             06:34:51 Sep 09, 2011 PDT
    payment_status           Completed
    address_status           confirmed
    payer_status             verified
    first_name               John
    last_name                Smith
    payer_email              buyer@paypalsandbox.com
    payer_id                 TESTBUYERID01
    address_name             John Smith
    address_country          United States
    address_country_code     US
    address_zip              95131
    address_state            CA
    address_city             San Jose
    address_street           123, any street
    business                 seller@paypalsandbox.com
    receiver_email           seller@paypalsandbox.com
    receiver_id              TESTSELLERID1
    residence_country        US
    item_name                something
    item_number              AK-1234
    quantity                 1
    shipping                 3.04
    tax                      2.02
    mc_currency              USD
    mc_fee                   0.44
    mc_gross                 12.34
    mc_gross_1               9.34
    txn_type                 web_accept
    txn_id                   51991334
    notify_version           2.1
    custom                   xyz123
    charset                  windows-1252
    verify_sign              Ah5rOpfPGo5g6FNg95DMPybP51J5AUEdXS1hqyRAP6WYYwaixKNDgQRR
