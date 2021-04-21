# Authenticate

Learn how to attack authentication mechanisms used in web applications

[Authenticate](https://tryhackme.com/room/authenticate)

## Topic's

- Brute Force (http-post-form)
- Re-registration
- JSON Web Token

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Deploy the VM

In today's time, the use of the authentication system is increasing because of the increase in the number of services that are coming up on the internet. But not everyone knows how to either make proper authentication software or how to properly set up one.

The aim of this room is to teach how to find authentication bugs and how you can exploit them.

1. Deploy the VM

`No answer needed`

## Task 2 Dictionary attack

The very obvious method of attacking any login form is just to brute force the credentials. But in this kind of brute force, we don't simply try numbers or simple alphabets. What we do is take an existing dictionary of commonly used username/passwords and use those to see if we can find the right combination. This is known as **Dictionary Attack**.

To perform a dictionary attack we can use a lot of tools like Hydra or Medusa but the issue with these CLI tools is that we need to provide a lot of arguments to them started and that could be confusing. That is why when trying a dictionary attack on a web application/form it's better to use Burp Suite. In Burp we can capture the login request and then use intruder to perform the attack.

If you are not familiar with burp suite then I would recommend that you first complete the **Learn Burp Suite** room.
Now let me show you an example using the Burp Suite:

1. Connect on port 8888
2. Now while the **Capture** is **On** in burp suite, enter any values you like in the username and password field.
3. Send this request to the intruder and for the position of the payload, we are just going to guess the password for the user jack. For payload, you can use any know default password list or maybe load a part of rockYou.

Note: Here I know that there exists a user named jack and that is why I am using that. In a real-life scenario, you might have to guess both the username and password.

4. Start the attack and wait for a bit. If you did everything correctly you'll notice that one of the requests sent by an intruder will have a bigger response then all of the others.

![](https://i.imgur.com/yv973xd.png)

As you can see in the above screenshot that on the 5th request the length value is **530** and the length of the content in other requests is 480. This could mean that the burp was able to successfully login in **Jack's** account using the password **12345678**.

1. What is the flag you found after logging as Jack?

`fad9ddc1feebd9e9bca05f02dd89e271`

2. Now try the same thing for username mike.

`No answer needed`

3. What is the flag you found after logging as Mike?

```
kali@kali:~/CTFs/tryhackme/Authenticate$ hydra -l mike -P /usr/share/wordlists/rockyou.txt "http-post-form://10.10.7.33:8888/login:user=^USER^&password=^PASS^:Invalid"
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-11-07 16:55:08
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-post-form://10.10.7.33:8888/login:user=^USER^&password=^PASS^:Invalid
[8888][http-post-form] host: 10.10.7.33   login: mike   password: 12345
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-11-07 16:55:17
```

`e1faaa144df2f24aa0a9284f4a5bb578`

## Task 3 Re-registration

In the previous task, we saw that it is possible to just simply guess/brute force the password with the help of a password dictionary. In this task, we are going to focus on a vulnerability that is unique in its own way.

A lot of times what happens is that developer forgets to sanitize the input(username & password) given by the user in the code of their application which can make them vulnerable to things like SQL injection but SQLi could be a bit difficult to exploit. So we are going to focus on a vulnerability that happens because of a developer's mistake but is very easy to exploit i.e re-registration of an existing user.

Let's understand this with the help of an example, say there is an existing user with the name **admin** and now we want to get access to their account so what we can do is try to re-register that username but with slight modification. We are going to enter " admin"(notice the space in the starting). Now when you enter that in the username field and enter other required information like email id or password and submit that data. It will actually register a new user but that user will have the same right as normal admin. And that new user will also be able to see all the content present under the user **admin**.

To see this in action go to port **8888** and try to register a user name **darren**, you'll see that user already exists so then try to register a user " darren" and you'll see that you are now logged in and will be able to see the content present only in Darren's account which in our case is the flag that you need to retrieve.

1. What is the flag that you found in darren's account?

`fe86079416a21a3c99937fea8874b667`

2. Now try to do the same trick and see if you can login as arthur.

`No answer needed`

3. What is the flag that you found in arthur's account?

`d9ac0f7db4fda460ac3edeb75d75e16e`

## Task 4 JSON Web Token

JSON Web Token(JWT) is one of the commonly used methods for authorization. This is a kind of cookie that is generated using HMAC hashing or public/private keys. So unlike any other kind of cookie, it lets the website know what kind of access the currently logged in user has. The only special thing about JWT is that they are in JSON format(after decoding).

JWT can be divided into 3 parts separated by a dot(.)

1. **Header**: This consists of the algorithm used and the type of the token.

`{ "alg": "HS256", "typ": "JWT"}`

alg could be HMAC, RSA, SHA256 or can even contain None value.

2. **Payload**: This is part that contains the access given to the certain user etc. This can vary from website to website, some can just have a simple username and some ID and others could have a lot of other details.
3. **Signature**: This is the part that is used to make sure that the integrity of the data was maintained while transferring it from a user's computer to the server and back. This is encrypted with whatever algorithm or alg that was passed in the header's value. And this can only be decrypted with a predefined secret(which should be difficult to)

Now to put all the 3 part together we base64 encode all of them separated by a dot(.) so it would look something like:

`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`

**Note**: This example was taken from [jwt.io](https://jwt.io/#debugger-io) and you should check that website out if you want to learn more about JWT.

**Exploitation**

If used properly this is a very secure way of authorization but the problem is with using is "properly". A lot of developers misconfigure their system leaving it open to exploitation.

Now one of the methods to exploit this is to perform a brute force/dictionary attack and find the secret used for encrypting the JWT token and then used that to generate new tokens. But here we are not going to do that, we are going to see a very amazing way of exploiting this.

If you remember, in the Header section I said that the **alg** can be whatever the algorithm is used and also it can be **None** if no encryption is to be used. Now, this should not be used when the application is in production but again the problem of misconfiguration comes in and make the application vulnerable to this kind of attack. The attack is that an attacker can log in as low privilege user says **guest** and then get the JWT token for that user and then decode the token and edit the headers to use set **alg** value to **None**. This would mean that no encryption has to be used therefore the attacker wouldn't need to the secret used for encryption.

**Practical**

Let's see this method in practice. For this challenge visit the port 5000.

It is a very simple login page and in that, you can log in via two users: user and user2. Now first let's try to login with the credentials of **user:user** . To do so first enter those credentials then click on the **Authenticate** button and then enable the capture in burp suite and then click on **the Go** button. In the burp tab, you should see a request to **/protected** and there you'll see the JWT token.

![](https://i.imgur.com/sLKHY9V.png)

Now take this JWT token and then you can decode it part by part.

So if we decode the first part, which will do: `{"typ":"JWT","alg":"HS256"}`

and decoding the 2nd part, we will get: `{"exp":1586620929,"iat":1586620629,"nbf":1586620629,"identity":1}`

If you try to decode the 3rd part then you'll get some gibberish. But that is okay we only need the first and the second part.

Now if we notice the **identity** value that is probably being used to identify the user but if you'll just edit that then it won't work because as I said the 3rd part is encrypted. So to bypass this we will make changes in the header as well as the value of the identity.

Encode the following string with base64 and that will be our first part

`{"typ":"JWT","alg":"NONE"}`

For the second part, we'll encode the following string: `{"exp":1586620929,"iat":1586620629,"nbf":1586620629,"identity":2}`

Notice how we changed the value of **identity** from **1** to **2**.
Since we placed the alg value to None we don't have to add a 3rd part or the encrypted value so we can just put a dot(.) after 2nd part and leave it like that. So the final string would look like:

`eyJ0eXAiOiJKV1QiLCJhbGciOiJOT05FIn0K.eyJleHAiOjE1ODY3MDUyOTUsImlhdCI6MTU4NjcwNDk5NSwibmJmIjoxNTg2NzA0OTk1LCJpZGVudGl0eSI6MH0K.`

Now open the developer's tools in your browser and edit the stored cookie of the website to this new one and then just press the Go button and you'll notice that it will prompt "Welcome user2: guest2".

In a similar manner, you can try to play and find other users on the website.

This kind of misconfiguration in the authentication system is common and could be exploited to escalate privileges or steal information.

1. Use the same method to find identity of admin user and retrieve the flag?

```
kali@kali:~/CTFs/tryhackme/Authenticate$ ./jwt.sh 0
Welcome admin: 92498880383088033228
```

`92498880383088033228`

## Task 5 No Auth

In this I am going to show you how a lot of systems don't even have proper authentication and their system is just left open for anyone to exploit it.

A lot of time on websites we see that when we register a user and login with our credentials we are given a certain id which either is completely a number or ends with a number. Most of the time developers secures their application but sometime in some places, it could happen that just by changing that number we are able to see some hidden or private data.

To test this go to port **7777**. On that just create an account. Once the account is created visit your **Private Space**.

![](https://imgur.com/RzRzvpx)

As you can see in the image above the URL have **/users/1**. Try to change that value to 2 and we will get access to the admin account

![](https://i.imgur.com/TAkXZ4b.png)

The chance of finding this kind of vulnerability is very low but it could be a very serious bug if you get lucky and found something like this.

1. Find the way to get into superadmin ad

`No answer needed`

2. What is the password for superadmin account?

```
kali@kali:~/CTFs/tryhackme/Authenticate$ curl http://10.10.7.33:7777/users/0
<!DOCTYPE html>
<html>
<head>

<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>No Auth</title>

<link href="/static/css/bootstrap.min.css" rel="stylesheet">
<link href="/static/css/styles.css" rel="stylesheet">

</head>

<body>


<div class="row">
        <div class="col-lg-7 col-sm-offset-2">
                <div class="panel panel-default">
                        <div class="panel-heading">Hello superadmin!</div>
                        <div class="panel-body">
                                <div class="col-md-6">
                                        <form method="post" action="/home" enctype="multipart/form-data">
                                                        <div class="form-group">
                                                        <p><h3>Your password: </h3>abcd1234</p>
                                                        <p><h3>Your secret data: </h3>Here&#39;s your flag: 72102933396288983011</p>
                                        </div>
                                </div>
                                </form>

                        </div>
                </div>
        </div><!-- /.col-->
</div><!-- /.row -->


        <script src="/static/js/jquery-1.11.1.min.js"></script>
        <script src="/static/js/bootstrap.min.js"></script>
</body>

</html>
```

`abcd1234`

3. What is the flag you found in superadmin account?

`72102933396288983011`
