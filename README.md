# Week7-Research-AlaaSahloub


## HTTP vs HTTPS
 
 
### How does HTTPS work? What are TLS/SSL certificates?
- HTTPS is a secure version of HTTP. A protocol to transfer the data with TLS to encrypt the data transferred when client and server communication. 

- The TLS and SSL are certificates installed in the server to create a secure connection between the server and client to transfer information and data. The TLS is a modern version of SSL and most secure.
 
### Why is this important to implement in your projects?
- HTTPS uses SSL/TLS that encrypts the communication so attackers can’t see or steal the data transferred between server and client.

- It will hide username and passwords and payment and bank data.
 
### Demo how to generate certificates and use them in a node project
- You will login to your SSL.
- Generate a private key.
- Create CSR(certificate signing request ) using private key.
- Generate the SSL certification from CRS.
- Then you will assign the key and cert to your server:

```js
  Const sslserver = https.createServer({[
      Key : fs.readFileSync(path.join(__dirname,’key.pem’)),
      Cert:fs.readFileSync(path.join(__dirname,’cert.pem’)),
      app
    })
  sslserver.listen(3000)
```


## Stateless vs stateful authentication
 
### What is session based authentication (stateful) vs token based authentication (stateless)?
#### session based authentication : 
- server will create session for the user after login and session id will store in the cookie browser 'client'. when the user still login the server will compare the session id coming from the browser with the session data stored in the server.

#### token based authentication : 
- when the user sends the login data the server creates JWT and secret and sends them to browser "will stored usually in the local storage" in the header. Then the browser will send an authentication request here the server will check the signature for JWT and get user information JWT.
 
### flow diagrams to show the steps involved in each process
 
- session based authentication (stateful) 
![](https://miro.medium.com/max/700/1*Hg1gUTXN5E3Nrku0jWCRow.png)
 
- token based authentication (stateless)
![](https://miro.medium.com/max/2400/1*PDry-Wb8JRquwnikIbJOJQ.png)
 
### What are the advantages and disadvantages of each?

#### token based authentication(Stateless)
  - advantages
    - Lower server overhead: in this way the data didn't store in the server so when you need to access the data you don't need to access the server when you have a huge number of data.

    - Easy to scale: In token based authentication you have one private key so each server has the same key that is less complicated.

    - Good to integrate with 3rd party application: In stateless authentication the 3rd party application will use the browser to communicate so if the application uses a 3rd party each user will use a secret key to communicate that is less the work with the server.
    
  - disadvantages
    - Can't revoke the session anytime: Since you create the session and store it on the browser the server can't delete it until expiration time.

    - Relatively complex to implement for one-session-server scenario: There is a difficulty to specify a one token to one user. One token can used by more one user.

    - Session data can't be changed until its expiration time: You can't change the token data until the expiration time so the client can make requests to the old session until it's end then can add a property. 
 
#### session based authentication
  - advantages
    - Revoke the session anytime: You can revoke the session anytime by making a request to the server and revoke it anytime.

    - Easy to implement and manage for one-session-server scenario: each user has a session id and no one can use another user's id.

    - Session data can be changed later: user can change the session data anytime.

  - disadvantages
    - Increasing server overhead: By each login the request will store the data inside the server memory so a large number of logins will be pressure on the server.

    - Fail to scale: If the session is distributed on different servers you need to track the user session to it's session server. That's because the data stored in the server and with a large number of users and it will be complex.

    - Difficult for 3rd party applications to use your credentials: Because of the data stored in the server so if you use 3rd party apps you need to verify your user's session. Here you will face more work on the server.



## Session-management in Express

### What are sessions?
  - Session is storing the user data on the server side, passing the session id and password in each http request. Any interaction on the single website recorded in the web session

### What are the different ways of managing sessions in express?
  - express session
  ```js
    const session = require('express-session')

    app.use(session({
      secret: 'keyboard cat',
      resave: false,
      saveUninitialized: true,
      cookie: { secure: true }
    }))
  ```

  - client session
  ```js
    const sessions = require("client-sessions")
    
    app.use(sessions({
      cookieName: 'mySession', 
      secret: 'blargadeeblargblarg',
      duration: 24 * 60 * 60 * 1000, 
      activeDuration: 1000 * 60 * 5 
    }))
  ```

  - cookie session
  ```js
    const cookieSession = require('cookie-session')
    
    app.use(cookieSession({
      name: 'session',
      keys: ['key1', 'key2']
    }))
  ```

### Create a minimal example of how to set up a session (FYI: pseudo code is fine)
  ```js
    const express = require('express');
    const session = require('express-session');
    const app = express();
    app.use(session({secret: 'secretkey'}));
  ```



## Attacks

### What are the following types of attack?

#### Man In The Middle (MITM)

![](https://www.imperva.com/learn/wp-content/uploads/sites/13/2017/09/man-in-the-middle-mitm-attack.png)

- term for when a perpetrator positions himself in a conversation between a user and an application—either to eavesdrop or to impersonate one of the parties, making it appear as if a normal exchange of information is underway. The goal of an attack is to steal personal information, such as login credentials, account details and credit card numbers. 

- how to defend Man In The Middle?
  - Using a VPN(virtual private network)
  - Only visiting HTTPS websites
  - Watching out for phishing scams


#### Cross Site Scripting (XSS)

![](https://www.imperva.com/learn/wp-content/uploads/sites/13/2016/03/reflected-cross-site-scripting-xss-attacks.png)

- is a type of security vulnerability typically found in web applications. XSS attacks enable attackers to inject client-side scripts into web pages viewed by other users. A cross-site scripting vulnerability may be used by attackers to bypass access controls such as the same-origin policy. 

- how to defend Cross Site Scripting?
  - Filter input on arrival
  - Encode data on output
  - Use appropriate response headers
  - Content Security Policy


#### Cross Site Request Forgery (CSRF)

![](https://www.imperva.com/learn/wp-content/uploads/sites/13/2019/01/csrf-cross-site-request-forgery.png)

- is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform. can be devastating for both the business and user. 

- how to defend Cross Site Request Forgery?
    - include a CSRF token within relevant requests. The token should be:
      - Tied to the user's session.
      - Strictly validated in every case before the relevant action is executed.


