---
layout: post
title: Building an API with Deno
description: Step by step instructions on how to create an API server using Deno
image: assets/images/deno.jpeg
tags: deno javascript
comments: true
noindex: true
---

## Introduction
The following text contains a tutorial to create an API server using Deno. [Deno](https://deno.land) is a programming language designed by Ryan Dahl, the creator of Node.JS. Ryan created Deno to solve common issues in Node.JS which couldn't be solved without an entirely new language. Specifically, Deno was created with the goal of making a secure runtime for JavaScript and TypeScript. To achieve these goals, Ryan built a language with the same syntax as Javascript/Typescript using Rust and V8, and dubbed it Deno (an anagram of Node). Deno's secure runtime and great developer experience, make it an awesome choice for your next project. By the end of this guide, you will have built a job-board API which can be plugged into a frontend.
 

## Tutorial

Install Deno

```
curl -fsSL https://deno.land/x/install/install.sh | sh -s v0.38.0
```

Check if Deno is installed on your system

```
deno --help
```
If its not you will have to add it to your [PATH](https://github.com/denoland/deno_install)

Ready to create our first Deno file!

```
touch api.ts
```

Importing packages from deno is a bit different than NPM.

This is how to we import Deno server


```javascript
import { serve } from "https://deno.land/std@v0.42.0/http/server.ts";
import { serve } from "https://deno.land/std@v0.42.0/http/server.ts";
```

And SQLite for our database
```javascript
import { open, save } from "<https://deno.land/x/sqlite/mod.ts>";
```

Hello world, we are running a server!
You should always create something simple in the beginning like this to test.

```javascript
const PORT = 8162;
const s = serve({ port: PORT });

console.log(` Listening on <http://localhost>:${PORT}/`);

for await (const req of s) {
  req.respond({ body: "Hello World\\n" });
}
```
Now run your application with

```
deno api —allow-net
```
Allow net flag allows Deno to network access

This is another difference between node and a strength of Deno. It only gets access to what you allow it to.


And visit localhost:8162

When you visit localhost:8162 You should see your page served!


Lets play further around with the request string

```javascript
for await (const req of s) {
  const url = req.url;

  req.respond({ body: `Hi there, accessing from ${url}` });
}
```

Now you can visit any url inside of your website try it:

localhost:8162/hello/world

The req.url variable is what we will all of our paths on.

Lets split it on question mark to give us URL params and the actual request URL. That will allow us to have things like count of records the api is supposed to return from the database.
```javascript
const params = req.url.split("?");
lets quickly check if there's something weird with the request

 if (params.length > 2) {
    req.respond(bad_request);
    continue;
  }
```

This means the user specified more than one question mark, we wont support that so we will send the bad request response

We are in a for loop, so just continue to skip all the other code in this iteration after we send the error response

Lets declare it above so its faster to error the request later.
```javascript
 const bad_request = { body: "Error 400 bad request.", status: 400 };
```
Lets get the search parameters using deno's inbuilt urlSearchParams class
```javascript
const url = params[0];
const search_params = new URLSearchParams(params[1]);
```

Now we are ready to start adding to our database


lets simply open the database file
```javascript
const db = await open("jobboard.db");
```


This means now Deno will need access to writing into files. Now we need to start it with a --allow-write flag as well.

```
deno api --allow-net --allow-write
```

We will be using our API to get job postings as well as add them.

So lets create those tables if they don't exist simply with

```javascript
db.query(
  "CREATE TABLE IF NOT EXISTS jobs (id INTEGER PRIMARY KEY AUTOINCREMENT, created_at DATETIME, last_updated DATETIME , active BOOLEAN, company_id INTEGER, apply_url TEXT, job_title CHARACTER(140), details TEXT, pay_range TEXT)"
);
db.query(
  "CREATE TABLE IF NOT EXISTS companies (id INTEGER PRIMARY KEY AUTOINCREMENT,  logo_url TEXT, name CHARACTER(50), description TEXT, created_at DATETIME, last_updated DATETIME , active BOOLEAN)"
);
```

Now we have our database setup

Lets create a path to view and a path to add job postings

doing a simple if statement on the url inside the request response loop
```javascript
 if (url == "/api/v1/jobs") {
}
```

Then check if for count parameter inside the html query. And if it is, check if its higher than 100. Which is the max records i want to allow users to query at a time.
```javascript
let count = 10; // base  
let request_count = search_params.get("count");
  if (request_count) {
    // enforcing max 100 record request
    if (parseInt(request_count) > 100) {
      req.respond(bad_request);
      continue;
    } else {
      count = parseInt(request_count);
    }
  }
```
Query the database for job_title and name of the company by joining two of our tables on the company id. Since a company can have multiple job postings, and a job posting can have only one company, we keep them in separate tables.

```javascript
for (const [
      job_title,
      name,
    ] of db.query(
      "SELECT job_title,name FROM jobs JOIN companies ON company_id = companies.id ORDER BY jobs.id DESC LIMIT ?",
      [count]
    )) {
      results.push({ company_name: name, job_title: job_title });
    }
```
And respond to the request
```javascript
req.respond({
      body: JSON.stringify(results),
      status: 200,
    });
    continue;
Code of the whole route :

if (url == "/api/v1/jobs") {
    // base values
    let count = 10; // 10 records

    // safe type get count param
    let request_count = search_params.get("count");
    if (request_count) {
      // enforcing max 100 record request
      if (parseInt(request_count) > 100) {
        req.respond(bad_request);
        continue;
      } else {
        count = parseInt(request_count);
      }
    }
    const results = [];
    for (const [
      job_title,
      name,
    ] of db.query(
      "SELECT job_title,name FROM jobs JOIN companies ON company_id = companies.id ORDER BY jobs.id DESC LIMIT ?",
      [count]
    )) {
      results.push({ company_name: name, job_title: job_title });
    }
    req.respond({
      body: JSON.stringify(results),
      status: 200,
    });
    continue;
  }
```

Now its time to implement a password protected route to add jobs to the jobs-board

```javascript
 if (url == "/api/v1/jobs/add") {
}
```
We can quickly hard code a password and make it more complex later, inside the Deno file itself. If the user provided ?pw URL param we check if its valid

```javascript
let password_valid = false; // initialize to false
  const password = "supersecurepassword"; // set our pasword
  let request_password = search_params.get("pw"); // check the search param pw if its a valid password
  if (request_password) {
    if (request_password == password) {
      password_valid = true;
    } 
  }
```

If its not we respond "Not Allowed" and close the connection.

```javascript


 if (password_valid == false) {
      req.respond({
        body: "Not Allowed",
        status: 405,
      });
      continue;
    }
```

Now that we identified the validity we can safely add the records from provided params.
Note - There is no way to know the ID of the company without query the company table with the name of it.

```javascript
const apply_url = search_params.get("apply_url");
      const job_title = search_params.get("job_title");
      const company = search_params.get("company");
      const details = search_params.get("details");
      const pay_range = search_params.get("pay_range");
      let company_id;
      // To get the company ID we need to know if from the other table
      for (const [id] of db.query("SELECT id FROM companies WHERE name = ?", [
        company,
      ]))
        company_id = id;
			// companies are added in a different endpoint
      if (!company_id) {
        req.respond({ body: "Company Name not specified.", status: 400 });
        continue;
      }
			// write into database
      db.query(
        "INSERT INTO jobs (company_id, apply_url, job_title, details, pay_range,created_at,last_updated,active) VALUES (?,?,?,?,?,?,?,?)",
        [company_id, apply_url, job_title, details, pay_range, time, time, 1]
      );
      req.respond({
        status: 200,
      });
```

Code of the whole password protected path

```javascript
if (url == "/api/v1/jobs/add") {
    let password_valid = false;
    const password = "supesecuredpassword";
    let request_password = search_params.get("pw");
    if (request_password) {
      if (request_password == password) {
        password_valid = true;
      }
    }
    if (password_valid == false) {
      req.respond({
        body: "Not Allowed",
        status: 405,
      });
      continue;
    }
    // validated can add
    else {
      const apply_url = search_params.get("apply_url");
      const job_title = search_params.get("job_title");
      const company = search_params.get("company");
      const details = search_params.get("details");
      const pay_range = search_params.get("pay_range");
      let company_id;
      // To get the company ID we need to know if from the other table
      for (const [id] of db.query("SELECT id FROM companies WHERE name = ?", [
        company,
      ]))
        company_id = id;
      // companies are added in a different endpoint
      if (!company_id) {
        req.respond({ body: "Company Name not specified.", status: 400 });
        continue;
      }
      // write into database
      db.query(
        "INSERT INTO jobs (company_id, apply_url, job_title, details, pay_range,created_at,last_updated,active) VALUES (?,?,?,?,?,?,?,?)",
        [company_id, apply_url, job_title, details, pay_range, time, time, 1]
      );
      req.respond({
        status: 200,
      });
			continue;
    }
  }
```

API done! simply close the database at the end of the file

```javascript
await save(db);
db.close()
```