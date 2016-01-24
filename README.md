MagicURLNetwork
=================================

# Introduction

**MagicURLNetwork** is a Java library which includes [Mson](https://github.com/magiclen/MagicLenJSON "Mson") to support JSON data transfer through networks. To make an URL connection, you only need to know what you want to do and choose an appropriate request method(GET, POST, PUT, HEAD...). You don't need to understand how HTTP, HTTPS and FTP work, and don't need to use any streaming library by yourself, too. You can easily implement a RESTful client program or file uploading/downloading program with this library.

# Usage

## MagicURLNetwork Class

**MagicURLNetwork** class is in the *org.magiclen.magicurlnetwork* package. It is an abstract class.

You can use **GET**, **POST**, **SinglePOST**, **PUT**, **HEAD** and **DELETE** static methods in  **MagicURLNetwork** class to create different URL connections clearly.

### Initialize

You don't need to initialize **MagicURLNetwork** class. Just use its static methods to do what you want.

### Get texts or binary data(such as downloading a file)

You can use **GET** method to create an URL connection to get some data(texts, files, others...) from URL.

For example, to open Google home page,

    final MagicURLNetwork network = MagicURLNetwork.GET("https://www.google.com.tw");
    network.open();
    System.out.println(network.getResultAsString());

You can also use **setTargetFile** method to store the data from URL to your target file.

For example, to download a file,

    final MagicURLNetwork network = MagicURLNetwork.GET("https://wordpress.org/latest.zip");
    network.setTargetFile(new File("/home/magiclen/wp.zip"));
    network.open();
    System.out.println(network.getResultAsFile());

### Upload texts or binary data(such as uploading a file)

You can use **POST**, **SinglePOST** or **PUT** method to create an URL connection to upload some data(texts, files, others...) to URL. You have to set you data as parameters(**SinglePOST** or **PUT** can only have one paramter). Use **setParameter** to set your parameters.

For example, to get an Facebook access token via Facebook Graph API,

    final MagicURLNetwork network = MagicURLNetwork.POST("https://graph.facebook.com/oauth/access_token");
    network.setParameter("redirect_uri", "http://magiclen.org");
    network.setParameter("client_id", "1001270393278383");
    network.setParameter("client_secret", "39be779714b4b26d14bca37558b425d4");
    network.setParameter("grant_type", "client_credentials");
    network.setAcceptNot200HTTPResponseCode(true);
    network.open();
    System.out.println(network.getResultAsString());

Look, we use **setAcceptNot200HTTPResponseCode** method to accept to get the response body when the HTTP response code is not 200.

### Check the header from URL

You can use **HEAD** method to create an URL connection to get header information from URL.

For example, to get the header from my website,

    final MagicURLNetwork network = MagicURLNetwork.HEAD("http://magiclen.org/");
    network.open();
    System.out.println(network.getResultAsString());

### Delete resource through URL

If you are using REST API, you may need to make a HTTP DELETE request to delete some resources. You can use **DELETE** method to do this.

### Assign your authorization

If you need to send your authorization(such as API key) in header, you can use **setAuthorization** method to set authorization.

### Set User Agent(can be used to simulate a web broswer)

If you want to set User Agent in header, you can use **setUserAgent** method to set User Agent. You can also find some built-in User Agent in **UserAgents** class which is in **MagicURLNetwork** class.

For example, to simulate Android WebKit broswer to get my website,

    final MagicURLNetwork network = MagicURLNetwork.GET("http://magiclen.org");
    network.setUserAgent(MagicURLNetwork.UserAgents.ANDROID);
    network.open();
    System.out.println(network.getResultAsString());

### Set cookies

In many cases, you have to use cookies to maintain the status of your network connection. You can use **setCookie** method to do this.

### Set properties

If you need to send some properties in header, you can use **setProperty** method to set properties.

### Asynchronous usage - NetworkListener

**NetworkListener** is an interface in **MagicURLNetwork** class, it can help you to capture the state of **MagicURLNetwork** object when it is opened in different thread. You can use **setNetworkListener** method to set your **NetworkListener** instance.

For example,

    final MagicURLNetwork network = MagicURLNetwork.GET("https://wordpress.org/latest.zip");
    network.setTargetFile(new File("/home/magiclen/wp.zip"));
    network.setNetworkListener(new MagicURLNetwork.NetworkListener() {

        @Override
        public void onStarted() {

        }

        @Override
        public void onRunning(final boolean receiving, final long currentBytes, final long totalBytes) {
          System.out.printf("%s: %d/%d%n", receiving ? "Receiving" : "Sending", currentBytes, totalBytes);
        }

        @Override
        public void onFailed(final String message, final boolean attemptDisconnect) {
          System.err.println(message);
        }

        @Override
        public void onFinished(final JSONObject resultHeader, final Object result) {
          System.out.println(result);
        }

    });
    new Thread(() -> network.open()).start();

## Body Class

**Body** class is in the *org.magiclen.magicurlnetwork.parameters* package. It is an abstract class used for **MagicURLNetwork** object's parameters.

If you want to send some data(texts, files, others...) to server through URL, you have to put each of them in a body, so that it can be passed as a parameter.

There are several types of body already defined in **MagicURLNetwork** library. And you can use static methods in **Body** class to create them.

> StringBody: Body.STRING
>
> NumberBody: Body.NUMBER
>
> JSONBody: Body.JSON
>
> FileBody: Body.FILE
>
> ArrayBody: Body.ARRAY

# License

    Copyright 2015-2016 magiclen.org

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

# What's More?

Please check out our web page at

http://magiclen.org/java-network/
