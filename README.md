# Android-Multicert
A Custom Security Configuration Framework for Android.

## Why Custom Security Configuration?
Android allows a list of trusted CA certifications. But using self-signed certificates alone, or in conjunction with CA certificates is tedious and manual process. This framework fills the gap.

BENEFITS:
Increased security.
Reduced costs - Particularly useful in development.

DRAWBACKS:
Less flexibility - Every SSL certificate change requires a mandatory app update from Google Play store.

## Getting Started:
Use this framework as a module or a package; Make sure the framework is able to access Android's resources directory.

For getting the custom SSLSocketFactory with pre-defined settings through a singleton instance:

	SSLSocketFactory sslSocketFactory = SecurityConfiguration.getCustomSSlSocketFactoryInstance(applicationContext);

This custom SSLSocketFactory can be used to perform regular Http operations in one of the ways:

	HttpsUrlConnection urlConnection = (HttpsUrlConnection) url.openConnection();
	urlConnection.setSSLSocketFactory(sslSocketFactory);

Or to make a quick Https request and get InputStream:

	InputStream inputStream = SecurityConfiguration.makeRequest(applicationContext, url);

To over-ride default settings, work on Security-Configuration.XML file, after reading more about it below and fully understanding it.

The Default settings of this framework allows only CA certifications from Android’s Trust Store. This can be overridden to support different other configurations. Overriding default Security settings implies the Self-Signed certificates are being used alone or in combination, which requires to declare local keystore and password parameters in the security config file. Failure to do so will throw test and run time errors.

## The Security-Configuration.XML file and its variables:

This configuration file should be placed under resources (res) directory of android application. The default configuration is to support CA certificates only, which is configured with:
	<bool name="USING_CA_CERTIFICATES">true</bool>
    	<bool name="USING_SELF_SIGNED_CERTIFICATES">false</bool>
	
	These values can be changed based on the requirements. At least one of them must be true all the times. Accidentally misconfiguring with false for both the values doesn’t voids SSL connections and still uses default settings. But, if the Self-Signed Certificates has to be used, the following values should also be declared, which basically makes sense as configuring Self-Signed certificates is required if determined to use.
	Please read more about using self-signed certificates before configuring them, as one important pit-fall in using them, particularly for mobile applications is that it required an application update from play-store across all the instances, every time a certificate has to be changed.

	<string name="LOCAL_KEY_STORE" translatable="false">
		sdtstore.bks		// File name of local Key store, full path is not required.
	</string>
    	<string name="LOCAL_KEY_STORE_PASSWORD" translatable="false">
		Changeit // The password for Local Key Store, use encrypted password if possible.
	</string>

And for local data encryption,
	<string name="MODE_CRYPTO" translatable="false">AES</string>
    	<string name="MODE_BLOCK" translatable="false">CBC</string>
    	<string name="MODE_PADDING" translatable="false">PKCS7Padding</string>
    	<string name="HASH_ALGORITHM" translatable="false">SHA-256</string>
These settings might not be changed unless or otherwise required. Make sure to run test suite after making changes here, as this ensures that changes are viable and successful.

## Local KeyStore for Self-Signed Certificates:
The local KeyStore is important if Self-Signed certificates are used. Please refer to using Self-Signed certificates for more information about generating Key-Store. This KeyStore must be password protected, in BKS format, and stored under Assets (main/assets) directory of app. This file name must also be specified in Security Configuration XML file.

## Copyright

The MIT License (MIT)

Copyright (c) 2015 Kishore Banala

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
