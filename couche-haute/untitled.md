# 🇬🇧 Deno Introduction with Practical Examples

### Key Takeaways

* Deno is a JavaScript runtime built with security in mind.
* Deno provides many helper functions to streamline frequent operations.
* Deno supports non-transpiled TypeScript
* Deno strives to address some challenges with Node.js
* It’s incredibly easy to create web servers and manage files with Deno

## What is Deno?

Deno is a simple, modern, and secure runtime for JavaScript and TypeScript applications built with the Chromium V8 JavaScript engine and Rust, created to [avoid several pain points and regrets with Node.js](https://www.infoq.com/news/2018/12/deno-v8-typescript/).

Deno was originally announced in 2018 and reached 1.0 in 2020, created by the original Node.js founder Ryan Dahl and other [mindful contributors](https://github.com/denoland/deno/graphs/contributors).

The name DE-NO may seem odd until you realize that it is simply the interchange of NO-DE. The Deno runtime:

* Adopts security by default. Unless explicitly allowed, Deno disallows file, network, or environment access.
* Includes TypeScript support out-of-the-box.
* Supports[ top-level await](https://medium.com/javascript-in-plain-english/javascript-top-level-await-in-a-nutshell-4e352b3fc8c8).
* Includes built-in [unit testing](https://deno.land/manual/testing) and code formatting (deno fmt).
* Is compatible with browser JavaScript APIs: Programs authored in JavaScript without the Deno namespace and its internal features should work in all modern browsers.
* Provides a one-file executable bundler through deno bundle command which lets you share your code for others to run without installing Deno.

## Get Deno

The easiest way to install Deno is to use the `deno_install` scripts. You can do this on Linux or macOS with:

`curl -fsSL https://deno.land/x/install/install.sh | sh`

Windows users can leverage [Chocolatey](https://chocolatey.org/):

`choco install deno`

A successful install with  Linux looks like this:

![](https://res.infoq.com/articles/deno-introduction-practical-examples/en/resources/11image002-1603478801048.jpg)

**Note**: You may also need to [export the deno directories to make the ](https://stackoverflow.com/questions/61851774/how-to-install-deno-on-ubuntu)[deno](https://stackoverflow.com/questions/61851774/how-to-install-deno-on-ubuntu)[ command globally available](https://stackoverflow.com/questions/61851774/how-to-install-deno-on-ubuntu).&#x20;

There are [additional ways to install Deno](https://deno.land/manual/getting\_started/installation).

## Hands-on Deno

Simple Example

Deno uses `.js` and `.ts` file extensions (though it will run any JavaScript or TypeScript file regardless of extension). Our first example demonstrates how we can safely write a browser-based Deno application, a simple JavaScript program that prints the current date and time.

`// date_time.js`

`console.log(new Date());`

Let’s run the code:

`deno run date_time.js`

Results:

2020-07-10T02:20:31.298Z

### Example 2: Accessing the Command-line Arguments

With Deno, we can log command-line arguments with just one line of code:

`// first_argument.js`

`console.log(Deno.args[0]);`

## Running Deno Programs

The run command runs any Deno code. And if you’re stuck, run deno --help, or deno -h, to get help using denocommands.

![](https://res.infoq.com/articles/deno-introduction-practical-examples/en/resources/26image003-1603478800391.jpg)

Deno also provides a way to run programs from a local file, a URL, or by integrating with other applications in `the stdin` by using `"-"` (without quotes).

Running code from filename:

`deno run date_time.js`

Running code from a URL is nearly identical:

`deno run https://example.com/date_time.js`

Read code from stdin:

`echo "console.log('Hello Deno')" | deno run`&#x20;

### Deno Script Arguments

Deno provides the args property from the Deno  namespace (Deno.args) for accessing script arguments, this property returns an array containing the passed arguments at runtime.&#x20;

A simple example below outputs the argument for a program with Deno.args.

`// args.js`

`console.log(Deno.args, Deno.args.length);`

**Note**: In Deno, anything passed after the script name will be passed as an argument and not consumed as a Deno runtime flag.

## Creating a Web Server With Deno

Putting simplicity and security into consideration, Deno ships with some browser-related APIs which allows you to create a web server with little or no difference from a client-side JavaScript application, with APIs including `fetch()`, [Web Worker](https://deno.land/manual/runtime/workers) and `WebAssembly`.

You can create a web server in Deno by importing the http module from the [official repo](https://deno.land/std/http/). Although there are already many libraries out there, the Deno system has also provided a straightforward way to accomplish this.

### First Web Server Example

Follow these steps to create a simple web server with Deno:

1. Create a file, for example, server.js.

```
import { serve } from "https://deno.land/std/http/server.ts";
const server = serve({ port: 5000 });
console.log('Listening to port 5000 on http://localhost:5000');
for await (const server_request of server) {
  server_request.respond({ body: "Deno server created\n" });
}
```

1. Run the code from the terminal.\
   &#x20;`deno run --allow-net server.js`
2. Open your favorite browser and visit [http://localhost:5000/](http://localhost:5000/)

In this example,  `--allow-net` provides network access permission to our program, otherwise Deno will throw a `PermissionDenied` error. This example:

* Imports a module from the HTTP remote package.
* Uses serve() method to create the server that listens on port 5000.
* Loops through a promise to send the string "Deno server created\n" whenever any request is made to port 5000.

### Another Web Server Example: HTML

To create an HTML web server in Deno, we will first have our HTML files that will be served whenever a user visits the server. To do this, follow the steps below:

1. Create a folder for your project, for example, deno\_server.
2. Navigate to the folder from your terminal:\
   &#x20;cd `deno_server`

Create a file name `index.html`, and paste the following code into it:

```



    My First Deno Web page


    My First Deno Web page


```

1. Create a file named `server.js` and paste the following code into it:

```
import { serve } from "https://deno.land/std/http/server.ts";
const server = serve({ port: 5000 });
for await (const server_request of server) {
  const _file = await Deno.readFile('./index.html');
  const decoder = new TextDecoder()
  server_request.respond({ body: decoder.decode(_file) });
}
```

1. Go to your terminal and run the code:\
   &#x20;`deno run --allow_net --allow_read server.js`

![](https://res.infoq.com/articles/deno-introduction-practical-examples/en/resources/17image004-1603478799305.jpg)

**Note**: Because the code reads the index.html file and also performs a network operation, `--allow-net and --allow-read` are required for the code to run successfully.

This example:

* Stores the hostname in a variable
* Stores the port in a variable
* Creates a loop that waits for a request from the client (e.g, user’s browser), reads a file from the server, and then sends the HTML file in response.

## TypeScript in Deno?

Deno supports both JavaScript and TypeScript as its first-class languages, allowing you to leverage TypeScript source code directly in your Deno program without needing to first transpile to JavaScript.

A quick example of a TypeScript program in Deno:

```
// ask_details.ts
interface PersonDetails {
  name: string;
  age: number;
  phone: string;
}

function getDetails(details: any[]): PersonDetails {
  return {name: details[0], age: details[1], phone: details[2]};
}
console.log(getDetails(Deno.args));
// deno run ask_details.ts "John Bull" 53 "7676254544212"
```

## Deno Package Management

Package management has been one of the factors for Node.js popularity, however this has also been a major bottleneck to its code instability and vulnerabilities. Node.js also predates JavaScript having a standard module format by several years. Instead of providing a standard package management system like npm, Deno instead recommends using standard ES module imports for loading URL modules. When URL modules are imported for the first time, Deno downloads and caches the module and its dependencies for later use. This also means Deno does not provide global CommonJS module functions like require() which exist in Node.js.

### Example of Importing a URL Module in Deno

```
// imports the Deno standard datetime module
import { parseDate, parseDateTime } from 'https://deno.land/std/datetime/mod.ts'
parseDate("20-01-2019", "dd-mm-yyyy") // output : new Date(2019, 0, 20)
parseDate("2019-01-20", "yyyy-mm-dd") // output : new Date(2019, 0, 20)
```

This example:

* Imports `parseDate(), parseDateTime()` methods from the[ Deno standard ](https://deno.land/std@0.67.0/datetime)[DateTime ](https://deno.land/std@0.67.0/datetime)[module](https://deno.land/std@0.67.0/datetime)[ ](https://deno.land/std@0.67.0/datetime)from URL
* Downloads the module and saves it in a cache directory, if not already cached, as specified by the `DENO_DIR`, or the default system's cache directory if DENO\_DIR is not specified.

## Handling Files With Deno

The Deno FileSystem allows you to perform operating system tasks on files. Like other server programming languages and runtime, Deno also has quite a [good number of file handling methods](https://lyty.dev/deno/deno-file-handling.html) to read, write, append and remove files. Let's look at a few filesystem examples below.

### Reading from a File in Deno

The Deno namespace provides the open() method for reading files.&#x20;

Let's create a file, `sample.ts`, and paste the following code:

```
// open() asynchronously returns the Uint8Array buffer of the file.
const file = await Deno.open('./sample.txt');
// TextDecoder decodes the Uint8Array to unicode text
const decoder = new TextDecoder('utf-8');
// text is decoded and saved.
const text = decoder.decode(await Deno.readAll(file));
console.log(text);
```

First let's run the code without the `--allow-read` flag to see how Deno behaves by default:

`deno run sample.ts`

![](https://res.infoq.com/articles/deno-introduction-practical-examples/en/resources/22image005-1603478802394.jpg)

This example throws  a PermissionDenied error because we wanted to read a file without requesting permission, so let's re-run the example with the correct file access:

`deno run --allow-read sample.ts`

![](https://res.infoq.com/articles/deno-introduction-practical-examples/en/resources/6image006-1603478799845.jpg)

### Writing to a File in Deno

The `Deno.writeFile()` method provides asynchronous methods for file writing. Let's explore with an example:

```
//create a text encoder that encodes utf8 string to Uint8Array
const encoder = new TextEncoder();
// encodes your utf8 text
const data = encoder.encode("Writing to file in Deno is simple!\n");
// asynchronously write to file 
// creates or overwrites a file
await Deno.writeFile("./sample1.txt", data);                 
// set permissions on new file
await Deno.writeFile("./sample3.txt", data, {mode: 0o777});  
// append the data to the file
await Deno.writeFile("./sample4.txt", data, {append: true}); 
// check if the file exists, if not, do nothing
await Deno.writeFile("./sample2.txt", data, {create: false});
```

Run the Deno file writing example  with the `--allow-write` flag.

Let's read the `sample1.txt` file to confirm the write, the Deno namespace also provides `readTextFile` which can be easily used to read text files:

```
// Read the file to see if it truly writes.
console.log(await Deno.readTextFile('./sample1.txt'));
```

## Formatting Code

Unlike Node.js which uses formatter libraries like [Prettier](https://prettier.io/),  Deno includes an automatic formatter with the `deno fmt` command.

#### To format all JS/TS files in the current directory including nested directories

`deno fmt`

#### To format specific files

`deno fmt sample.txt sample1.txt`

#### To format strings from the stdin

`cat sample.ts | deno fmt -`

#### To check the file format status

`deno fmt --check`

#### To check the format status of some files, space-separated

`deno fmt --check sample.js sample.ts`

![](https://res.infoq.com/articles/deno-introduction-practical-examples/en/resources/16image007-1603478801844.jpg)

#### To ignore formatting a section or block of code, precede the code section with // deno-fmt-ignore comment. For example:

```
// deno-fmt-ignore comment
const arrs = ["I", "will", "be", "left","unformatted"
               ];
```

To ignore formatting for an entire file, add // deno-fmt-ignore-file to the top of the file:

```
// deno-fmt-ignore-file
const arrs = ["I", "will", "be", "left","unformatted"  
 ];
const rand = Math.ceil(Math.random() * arrs.length) - 1;
// display a random element
console.log(arrs[rand]);
```

## Comparisons With Node.js

Deno and Node.js have several key similarities and differences:

| **Environment**                        | **Deno**                                                                                                                                                                       | **Node.js**                              |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------- |
| **JavaScript engine and technologies** | **Built with Chrome V8 and Rust**                                                                                                                                              | **Built with Chrome V8 and C++**         |
| **Package management**                 | **Intentionally does not encourage a single standard package manager (although** [**the community is exploring some ideas**](https://deno.land/x?query=package%20manager)**)** | **npm**                                  |
| **Module loading**                     | **ES Modules**                                                                                                                                                                 | **CommonJS or ES Modules**               |
| **Default security restrictions**      | **Restricts access to a file, network, and environment access.**                                                                                                               | **Does not disallow access by default.** |

## Conclusion

Deno is still a very new environment with many promising features around security, speed, distribution, and language support. However, due to its infancy, Deno is not yet widely used or recommended for critical production applications. . Deno is open-source software available under the MIT license. Contributions are encouraged via the[ ](https://github.com/denoland/deno)[Deno Project](https://github.com/denoland/deno)and should follow the[ ](https://github.com/denolib/awesome-deno/blob/master/CONTRIBUTING.md)[Deno contribution guidelines](https://github.com/denolib/awesome-deno/blob/master/CONTRIBUTING.md).

Congratulations on your practical introduction to using Deno. Here are few more resources to become familiar with Deno:

* [Deno v1.0.0 released to solve Node.js design flaws](https://stackoverflow.blog/2020/05/22/deno-v1-0-0-released-to-solve-node-js-design-flaws/) — By Ryan Donovan
* [Deno Loves WebAssembly](https://www.infoq.com/articles/deno-loves-webassembly/)  — By Michael Yuan
* [Creating your first REST API with Deno and Postgres](https://blog.logrocket.com/creating-your-first-rest-api-with-deno-and-postgres/) — By Diogo Souza

Please share any questions or feedback in the comments.

## About the Author

![](https://res.infoq.com/articles/deno-introduction-practical-examples/en/resources/1erisan-1603480459910.jpg)**Erisan Olasheni** is a Full-stack software engineer, freelancer, and a creative thinker who loves writing programming tutorials and tech tips. CTO at siit.co.
