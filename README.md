# psql-api
RAPID RESTful API for PostgreSQL

INSTALLATION  ( install local is required  --save)

              npm install psql-api --save

Requirements:
- OS: Linux, Windows 
- NodeJS
- OpenSSL should be installed before in order to allow the server to create it's first self-signed SSL certificate.
- Tested with Posgresql 9.3 and up
- Linux - set your computer to accept opening lower ports without root access for NodeJs (80/443), next code works for Ubuntu 14.

               sudo apt-get install libcap2-bin
               sudo setcap cap_net_bind_service=+ep /usr/local/bin/node

sample new server.js code:

              server = require("psql-api");
              server.start();


Install from git as standalone server:

              git clone https://github.com/QBisConsult/psql-api.git

Run this inside the new server folder:   

               sudo npm install

Run the server with:

               node start.js

This server will open two ports, one for administration on HTTPS (443) and another for API requests.

The server can be managed at:  https://server_Ip_or_domain name

It will use a self-signed SSL certificate generated at first start until you will provide an authorized one or change with your own.

If OpenSSL can not be found, you will need to provide a SSL certificate otherwise the server will not start.
Put the certificate into "ssl" folder as service.crt and service.key files. The SSL certificate is a PEM encoded crt file.

You can change the server port and protocol (HTTP/HTTPS) for API access from settings page.

Please check the SSL documentation page for more information.

See SETTINGS chapter for this server SSL setup after installation.

FEATURES

Implements all CRUD operations (CREATE, READ, UPDATE, DELETE).
Automatically imports the database structure and create a metadata of your database.Single point access, accept POST/PUT/GET/DELETE http methods and respond with a JSON data object.

Basic operations can be used from the start:

    CREATE - insert records
    READ - read one record by ID
    UPDATE - update one record
    DELETE - delete one record

The servers accepts batch of different commands at once and uses transactions by default.

Inject your code BEFORE and AFTER operations in order to customize access to each action.

Create queries and access them with simple GET commands.

Current version can be set to access data from one PostgreSQL server.

Management of the database structure requires a third party application like pgAdmin or others. This API server will check requests before sent them to the database against a metadata created at start in order to avoid unnecessary traffic with the database server, the server will need to update it's metadata if database changes occurs. This API server will not alter the database structures and it is no plan to do that in the future.

This RESTful API server can be used to offer WEB services for various kind of applications and devices over HTTP/HTTPS protocol like WEB, mobile or IoT applications  that consumes WEB services.



License MIT

Code of the North - Updates

### Critical Information
- Almost any error will be logged to the terminal session where you start the server, unless you handle it otherwise. The errors flip past the screen quickly(in our experience), so you will need to scroll back to see the messages. It is possible to save a query or a token that may cause the application to throw errors. Make a single change, then open a new tab in the application, or make a new web request, then check the log for errors if you don't see what you expect. **If the RAPID web admin interface isn't showing your saved values, there are probably errors. When in doubt, delete all values from your rcfg.sqls and rcfg.tokens tables manually in postgres.** Note: reusing query names is tricky, haven't quite figured out where the query name is stored, but it's not in the aforementioned tables. 
- You must click on 'Settings' and set a Token Password
- You must create a token. Click on the icon to the right of the tabs in interface to create a new tab; select Tokens. Using the package at https://github.com/auth0/node-jsonwebtoken.git and the following code(saved to file and executed with "node <filename>"), generate the the token:

        var jwt = require('jsonwebtoken'),
        fs = require('fs');
        var cert = fs.readFileSync('private.key');  // get private key from file in the same directory naned private.key,          containing just the password on line 1
        var token = jwt.sign({ user: 'put your username here' }, cert, { algorithm: 'HS256'});
        console.log(token);

- You must use a saved query, ad-hoc SQL is not permitted for security reasons.
- You do not need to include a semi-colon at the end of your query when inputting it in the web GUI.
- You **do** need to save, run, test and validate a query before you can execute it with an HTTP request.

** Firefox REST client test **

1. Create a custom header
      Name: Authorization
      Value: <insert token from jwt code above>
2. URL: http://**<insert your ip address or hostname>**:3330/rapid/rpdquery?csql=**<insert name of saved query>**&limit=100&offset=0
3. USE HTTP GET for this test
4. You should populate your table with some test data, so your query returns results. 
