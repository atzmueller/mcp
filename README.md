<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-40README-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

# 40ants-mcp - The framework for building MCP servers and clients in Common Lisp.

<a id="40-ants-mcp-asdf-system-details"></a>

## 40ANTS-MCP ASDF System Details

* Description: The framework for building `MCP` servers and clients in Common Lisp.
* Licence: Unlicense
* Author: Alexander Artemenko <svetlyak.40wt@gmail.com>
* Homepage: [https://40ants.com/mcp/][7318]
* Bug tracker: [https://github.com/40ants/mcp/issues][6ed2]
* Source control: [GIT][e31f]
* Depends on: [alexandria][8236], [bordeaux-threads][3dbf], [cl-ppcre][49b9], [clack][482d], [clack-sse][b0a4], [jsonrpc][a9bd], [lack][63d7], [lack-request][6a02], [lack-response][521b], [local-time][46a1], [log4cl][7f8b], [log4cl-extras][691c], [openrpc-client][b8fd], [openrpc-server][c8e7], [sento][626d], [serapeum][c41d], [sse-server][0027], [trivial-gray-streams][588d], [uuid][d6b3], [yason][aba2]

[![](https://github-actions.40ants.com/40ants/mcp/matrix.svg?only=ci.run-tests)][04f0]

![](http://quickdocs.org/badge/40ants-mcp.svg)

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-40ABOUT-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

## About

A comprehensive framework for building [Model Context Protocol (MCP)][473e] servers in Common Lisp. This library provides a complete implementation of the `MCP` specification with an easy-to-use `API` for creating servers that can interact with `AI` assistants like Claude Desktop.

**Active development is ongoing and the interface is likely to change.**

<a id="features"></a>

### Features

* ✅ **`STDIO` Transport**: Native support for `STDIO`-based communication
* ✅ **Streamable`HTTP` Transport**: Allowing to create remotely hosted `MCP` servers
* ✅ **Tools System**: Register and execute custom tools with `JSON` Schema validation
* ✅ **Built on Open`RPC`**: Leverages the robust [40ants OpenRPC library][348e]
* ✅ **`CLOS`-based**: Object-oriented design with proper encapsulation
* ✅ **Easy Integration**: Simple `API` for adding functionality
* ✅ **Error Handling**: Comprehensive error management with proper `JSON-RPC` error codes
* ✅ **Interactive Editing**: `MCP` tools can be edited and updated on the fly, using `REPL` driven approach

<a id="roadmap"></a>

### Roadmap

* 🔄 **Full `MCP` Specification Support**: Complete implementation of `MCP` protocol version 2025-06-18
* 🔄 **Resources System**: Serve dynamic and static resources via `URI`
* 🔄 **Prompts System**: Provide prompt templates with argument interpolation
* 🔄 **`MCP` Client Protocol**: Implement client-side protocol for connecting to `MCP` servers

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-40INSTALLATION-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

## Installation

You can install this library from Quicklisp, but you want to receive updates quickly, then install it from Ultralisp.org:

```
(ql-dist:install-dist "http://dist.ultralisp.org/"
                      :prompt nil)
(ql:quickload :40ants-mcp)
```
<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-40USAGE-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

## Usage

Here's a quick example of how to create an `MCP` server with custom tools:

```lisp
(defpackage #:my-mcp-server
  (:use #:cl)
  (:import-from #:40ants-mcp/content/text
                #:text-content)
  (:import-from #:openrpc-server))

(in-package #:my-mcp-server)

;; Define your API
(openrpc-server:define-api (my-tools :title "My Custom Tools"))

;; Define a tool that adds two numbers
(40ants-mcp/tools:define-tool (my-tools add) (a b)
  (:summary "Adds two numbers and returns the result.")
  (:param a integer "First number to add.")
  (:param b integer "Second number to add.")
  (:result (soft-list-of text-content))
  (list (make-instance 'text-content
                      :text (format nil "The sum of ~A and ~A is: ~A"
                                  a b (+ a b)))))

;; Start the server
(40ants-mcp/server/definition:start-server my-tools)
```
<a id="running-as-a-script"></a>

### Running as a Script

For production use, you can create a Roswell script. Create a file `my-mcp.ros`:

```lisp
#!/bin/sh
#|-*- mode:lisp -*-|#
#|
exec ros -Q -- $0 \"$@\"
|#

(ql:quickload '(:40ants-mcp :alexandria) :silent t)

;; Your package and tool definitions here...

(defun main (&rest argv)
  (declare (ignore argv))
  (40ants-mcp/server/definition:start-server my-tools))
```
Build and run the script:

```bash
# Build the script
ros build my-mcp.ros

# Run the server
./my-mcp

# With remote debugging support
SLYNK_PORT=4005 ./my-mcp
```
Each tool you define should return a list of content items. The most common content type is `text-content`, but you can also return other types defined in the `MCP` specification.

For more examples, check the `examples/` directory in the source code.

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-40API-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

## API

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-4040ANTS-MCP-2FCONTENT-2FBASE-3FPACKAGE-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

### 40ANTS-MCP/CONTENT/BASE

<a id="x-28-23A-28-2823-29-20BASE-CHAR-20-2E-20-2240ANTS-MCP-2FCONTENT-2FBASE-22-29-20PACKAGE-29"></a>

#### [package](6eb7) `40ants-mcp/content/base`

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-7C-4040ANTS-MCP-2FCONTENT-2FBASE-3FClasses-SECTION-7C-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

#### Classes

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-4040ANTS-MCP-2FCONTENT-2FBASE-24CONTENT-3FCLASS-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

##### CONTENT

<a id="x-2840ANTS-MCP-2FCONTENT-2FBASE-3ACONTENT-20CLASS-29"></a>

###### [class](41cb) `40ants-mcp/content/base:content` ()

**Readers**

<a id="x-2840ANTS-MCP-2FCONTENT-2FBASE-3ACONTENT-TYPE-20-2840ANTS-DOC-2FLOCATIVES-3AREADER-2040ANTS-MCP-2FCONTENT-2FBASE-3ACONTENT-29-29"></a>

###### [reader](46e2) `40ants-mcp/content/base:content-type` (content) (:TYPE = "unknown")

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-4040ANTS-MCP-2FCONTENT-2FTEXT-3FPACKAGE-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

### 40ANTS-MCP/CONTENT/TEXT

<a id="x-28-23A-28-2823-29-20BASE-CHAR-20-2E-20-2240ANTS-MCP-2FCONTENT-2FTEXT-22-29-20PACKAGE-29"></a>

#### [package](e968) `40ants-mcp/content/text`

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-7C-4040ANTS-MCP-2FCONTENT-2FTEXT-3FClasses-SECTION-7C-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

#### Classes

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-4040ANTS-MCP-2FCONTENT-2FTEXT-24TEXT-CONTENT-3FCLASS-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

##### TEXT-CONTENT

<a id="x-2840ANTS-MCP-2FCONTENT-2FTEXT-3ATEXT-CONTENT-20CLASS-29"></a>

###### [class](ba3a) `40ants-mcp/content/text:text-content` (content)

**Readers**

<a id="x-2840ANTS-MCP-2FCONTENT-2FTEXT-3ACONTENT-TEXT-20-2840ANTS-DOC-2FLOCATIVES-3AREADER-2040ANTS-MCP-2FCONTENT-2FTEXT-3ATEXT-CONTENT-29-29"></a>

###### [reader](eec6) `40ants-mcp/content/text:content-text` (text-content) (:text)

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-4040ANTS-MCP-2FHTTP-TRANSPORT-3FPACKAGE-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

### 40ANTS-MCP/HTTP-TRANSPORT

<a id="x-28-23A-28-2825-29-20BASE-CHAR-20-2E-20-2240ANTS-MCP-2FHTTP-TRANSPORT-22-29-20PACKAGE-29"></a>

#### [package](bddb) `40ants-mcp/http-transport`

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-7C-4040ANTS-MCP-2FHTTP-TRANSPORT-3FClasses-SECTION-7C-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

#### Classes

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-4040ANTS-MCP-2FHTTP-TRANSPORT-24HTTP-TRANSPORT-3FCLASS-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

##### HTTP-TRANSPORT

<a id="x-2840ANTS-MCP-2FHTTP-TRANSPORT-3AHTTP-TRANSPORT-20CLASS-29"></a>

###### [class](d2f4) `40ants-mcp/http-transport:http-transport` ()

`HTTP` transport implementation for `MCP` (Model Context Protocol) communication.
This class handles `JSON-RPC` message exchange via `HTTP` `POST` requests.

**Readers**

<a id="x-2840ANTS-MCP-2FHTTP-TRANSPORT-3ATRANSPORT-LACK-APP-20-2840ANTS-DOC-2FLOCATIVES-3AREADER-2040ANTS-MCP-2FHTTP-TRANSPORT-3AHTTP-TRANSPORT-29-29"></a>

###### [reader](59d2) `40ants-mcp/http-transport:transport-lack-app` (http-transport) ()

Lack application instance

<a id="x-2840ANTS-MCP-2FHTTP-TRANSPORT-3ATRANSPORT-MESSAGE-HANDLER-20-2840ANTS-DOC-2FLOCATIVES-3AREADER-2040ANTS-MCP-2FHTTP-TRANSPORT-3AHTTP-TRANSPORT-29-29"></a>

###### [reader](5447) `40ants-mcp/http-transport:transport-message-handler` (http-transport) ()

Function to handle incoming messages

<a id="x-2840ANTS-MCP-2FHTTP-TRANSPORT-3ATRANSPORT-PORT-20-2840ANTS-DOC-2FLOCATIVES-3AREADER-2040ANTS-MCP-2FHTTP-TRANSPORT-3AHTTP-TRANSPORT-29-29"></a>

###### [reader](460e) `40ants-mcp/http-transport:transport-port` (http-transport) (:port = 8080)

Port number to listen on.

<a id="x-2840ANTS-MCP-2FHTTP-TRANSPORT-3ATRANSPORT-RUNNING-P-20-2840ANTS-DOC-2FLOCATIVES-3AREADER-2040ANTS-MCP-2FHTTP-TRANSPORT-3AHTTP-TRANSPORT-29-29"></a>

###### [reader](e960) `40ants-mcp/http-transport:transport-running-p` (http-transport) (= t)

Flag indicating if transport is active

<a id="x-2840ANTS-MCP-2FHTTP-TRANSPORT-3ATRANSPORT-SERVER-20-2840ANTS-DOC-2FLOCATIVES-3AREADER-2040ANTS-MCP-2FHTTP-TRANSPORT-3AHTTP-TRANSPORT-29-29"></a>

###### [reader](c7d8) `40ants-mcp/http-transport:transport-server` (http-transport) ()

Clack server instance

**Accessors**

<a id="x-2840ANTS-MCP-2FHTTP-TRANSPORT-3ATRANSPORT-LACK-APP-20-2840ANTS-DOC-2FLOCATIVES-3AACCESSOR-2040ANTS-MCP-2FHTTP-TRANSPORT-3AHTTP-TRANSPORT-29-29"></a>

###### [accessor](59d2) `40ants-mcp/http-transport:transport-lack-app` (http-transport) ()

Lack application instance

<a id="x-2840ANTS-MCP-2FHTTP-TRANSPORT-3ATRANSPORT-MESSAGE-HANDLER-20-2840ANTS-DOC-2FLOCATIVES-3AACCESSOR-2040ANTS-MCP-2FHTTP-TRANSPORT-3AHTTP-TRANSPORT-29-29"></a>

###### [accessor](5447) `40ants-mcp/http-transport:transport-message-handler` (http-transport) ()

Function to handle incoming messages

<a id="x-2840ANTS-MCP-2FHTTP-TRANSPORT-3ATRANSPORT-RUNNING-P-20-2840ANTS-DOC-2FLOCATIVES-3AACCESSOR-2040ANTS-MCP-2FHTTP-TRANSPORT-3AHTTP-TRANSPORT-29-29"></a>

###### [accessor](e960) `40ants-mcp/http-transport:transport-running-p` (http-transport) (= t)

Flag indicating if transport is active

<a id="x-2840ANTS-MCP-2FHTTP-TRANSPORT-3ATRANSPORT-SERVER-20-2840ANTS-DOC-2FLOCATIVES-3AACCESSOR-2040ANTS-MCP-2FHTTP-TRANSPORT-3AHTTP-TRANSPORT-29-29"></a>

###### [accessor](c7d8) `40ants-mcp/http-transport:transport-server` (http-transport) ()

Clack server instance

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-4040ANTS-MCP-2FSERVER-2FDEFINITION-3FPACKAGE-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

### 40ANTS-MCP/SERVER/DEFINITION

<a id="x-28-23A-28-2828-29-20BASE-CHAR-20-2E-20-2240ANTS-MCP-2FSERVER-2FDEFINITION-22-29-20PACKAGE-29"></a>

#### [package](e969) `40ants-mcp/server/definition`

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-7C-4040ANTS-MCP-2FSERVER-2FDEFINITION-3FClasses-SECTION-7C-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

#### Classes

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-4040ANTS-MCP-2FSERVER-2FDEFINITION-24MCP-SERVER-3FCLASS-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

##### MCP-SERVER

<a id="x-2840ANTS-MCP-2FSERVER-2FDEFINITION-3AMCP-SERVER-20CLASS-29"></a>

###### [class](158a) `40ants-mcp/server/definition:mcp-server` (api)

**Readers**

<a id="x-2840ANTS-MCP-2FSERVER-2FDEFINITION-3ASERVER-TOOLS-COLLECTIONS-20-2840ANTS-DOC-2FLOCATIVES-3AREADER-2040ANTS-MCP-2FSERVER-2FDEFINITION-3AMCP-SERVER-29-29"></a>

###### [reader](691e) `40ants-mcp/server/definition:server-tools-collections` (mcp-server) (collections = nil)

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-7C-4040ANTS-MCP-2FSERVER-2FDEFINITION-3FFunctions-SECTION-7C-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

#### Functions

<a id="x-2840ANTS-MCP-2FSERVER-2FDEFINITION-3ASTART-SERVER-20FUNCTION-29"></a>

##### [function](2c74) `40ants-mcp/server/definition:start-server` tools-collections &key (transport :stdio) (port 8080)

Start the `MCP` server with specified transport.
`TRANSPORT` can be :stdio or :http.
`PORT` is only used when transport is :http.

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-4040ANTS-MCP-2FSERVER-2FERRORS-3FPACKAGE-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

### 40ANTS-MCP/SERVER/ERRORS

<a id="x-28-23A-28-2824-29-20BASE-CHAR-20-2E-20-2240ANTS-MCP-2FSERVER-2FERRORS-22-29-20PACKAGE-29"></a>

#### [package](cd62) `40ants-mcp/server/errors`

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-7C-4040ANTS-MCP-2FSERVER-2FERRORS-3FClasses-SECTION-7C-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

#### Classes

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-4040ANTS-MCP-2FSERVER-2FERRORS-24TOOL-ERROR-3FCLASS-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

##### TOOL-ERROR

<a id="x-2840ANTS-MCP-2FSERVER-2FERRORS-3ATOOL-ERROR-20CONDITION-29"></a>

###### [condition](22c6) `40ants-mcp/server/errors:tool-error` ()

You should signal this error in case if the tool can't accomplish it's job.

**Readers**

<a id="x-2840ANTS-MCP-2FSERVER-2FERRORS-3ATOOL-ERROR-CONTENT-20-2840ANTS-DOC-2FLOCATIVES-3AREADER-2040ANTS-MCP-2FSERVER-2FERRORS-3ATOOL-ERROR-29-29"></a>

###### [reader](22c6) `40ants-mcp/server/errors:tool-error-content` (tool-error) (:content)

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-4040ANTS-MCP-2FSTDIO-TRANSPORT-3FPACKAGE-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

### 40ANTS-MCP/STDIO-TRANSPORT

<a id="x-28-23A-28-2826-29-20BASE-CHAR-20-2E-20-2240ANTS-MCP-2FSTDIO-TRANSPORT-22-29-20PACKAGE-29"></a>

#### [package](6786) `40ants-mcp/stdio-transport`

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-7C-4040ANTS-MCP-2FSTDIO-TRANSPORT-3FClasses-SECTION-7C-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

#### Classes

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-4040ANTS-MCP-2FSTDIO-TRANSPORT-24STDIO-TRANSPORT-3FCLASS-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

##### STDIO-TRANSPORT

<a id="x-2840ANTS-MCP-2FSTDIO-TRANSPORT-3ASTDIO-TRANSPORT-20CLASS-29"></a>

###### [class](aaf4) `40ants-mcp/stdio-transport:stdio-transport` ()

`STDIO` transport implementation for `MCP` (Model Context Protocol) communication.
This class handles `JSON-RPC` message exchange via standard input/output streams.
It is designed to work with the `MCP` protocol specification for `AI` model communication.

**Readers**

<a id="x-2840ANTS-MCP-2FSTDIO-TRANSPORT-3ATRANSPORT-INPUT-20-2840ANTS-DOC-2FLOCATIVES-3AREADER-2040ANTS-MCP-2FSTDIO-TRANSPORT-3ASTDIO-TRANSPORT-29-29"></a>

###### [reader](70f6) `40ants-mcp/stdio-transport:transport-input` (stdio-transport) (:input-stream = \*standard-input\*)

Input stream for reading `JSON-RPC` messages. Defaults to *standard-input*.

<a id="x-2840ANTS-MCP-2FSTDIO-TRANSPORT-3ATRANSPORT-OUTPUT-20-2840ANTS-DOC-2FLOCATIVES-3AREADER-2040ANTS-MCP-2FSTDIO-TRANSPORT-3ASTDIO-TRANSPORT-29-29"></a>

###### [reader](ac77) `40ants-mcp/stdio-transport:transport-output` (stdio-transport) (:output-stream = \*standard-output\*)

Output stream for writing `JSON-RPC` responses. Defaults to *standard-output*.

<a id="x-2840ANTS-MCP-2FSTDIO-TRANSPORT-3ATRANSPORT-RUNNING-P-20-2840ANTS-DOC-2FLOCATIVES-3AREADER-2040ANTS-MCP-2FSTDIO-TRANSPORT-3ASTDIO-TRANSPORT-29-29"></a>

###### [reader](0bd0) `40ants-mcp/stdio-transport:transport-running-p` (stdio-transport) (= t)

Flag indicating if transport is active and processing messages.

**Accessors**

<a id="x-2840ANTS-MCP-2FSTDIO-TRANSPORT-3ATRANSPORT-INPUT-20-2840ANTS-DOC-2FLOCATIVES-3AACCESSOR-2040ANTS-MCP-2FSTDIO-TRANSPORT-3ASTDIO-TRANSPORT-29-29"></a>

###### [accessor](70f6) `40ants-mcp/stdio-transport:transport-input` (stdio-transport) (:input-stream = \*standard-input\*)

Input stream for reading `JSON-RPC` messages. Defaults to *standard-input*.

<a id="x-2840ANTS-MCP-2FSTDIO-TRANSPORT-3ATRANSPORT-OUTPUT-20-2840ANTS-DOC-2FLOCATIVES-3AACCESSOR-2040ANTS-MCP-2FSTDIO-TRANSPORT-3ASTDIO-TRANSPORT-29-29"></a>

###### [accessor](ac77) `40ants-mcp/stdio-transport:transport-output` (stdio-transport) (:output-stream = \*standard-output\*)

Output stream for writing `JSON-RPC` responses. Defaults to *standard-output*.

<a id="x-2840ANTS-MCP-2FSTDIO-TRANSPORT-3ATRANSPORT-RUNNING-P-20-2840ANTS-DOC-2FLOCATIVES-3AACCESSOR-2040ANTS-MCP-2FSTDIO-TRANSPORT-3ASTDIO-TRANSPORT-29-29"></a>

###### [accessor](0bd0) `40ants-mcp/stdio-transport:transport-running-p` (stdio-transport) (= t)

Flag indicating if transport is active and processing messages.

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-7C-4040ANTS-MCP-2FSTDIO-TRANSPORT-3FGenerics-SECTION-7C-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

#### Generics

<a id="x-2840ANTS-MCP-2FTRANSPORT-2FBASE-3ARECEIVE-MESSAGE-20GENERIC-FUNCTION-29"></a>

##### [generic-function](c339) `40ants-mcp/transport/base:receive-message` transport

Receive a `JSON-RPC` message, returns a message or `NIL`.

<a id="x-2840ANTS-MCP-2FTRANSPORT-2FBASE-3ASEND-MESSAGE-20GENERIC-FUNCTION-29"></a>

##### [generic-function](0897) `40ants-mcp/transport/base:send-message` transport message

Send a `JSON-RPC` message, returns no values.

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-4040ANTS-MCP-2FTOOLS-3FPACKAGE-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

### 40ANTS-MCP/TOOLS

<a id="x-28-23A-28-2816-29-20BASE-CHAR-20-2E-20-2240ANTS-MCP-2FTOOLS-22-29-20PACKAGE-29"></a>

#### [package](1aa4) `40ants-mcp/tools`

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-7C-4040ANTS-MCP-2FTOOLS-3FMacros-SECTION-7C-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

#### Macros

<a id="x-2840ANTS-MCP-2FTOOLS-3ADEFINE-TOOL-20-2840ANTS-DOC-2FLOCATIVES-3AMACRO-29-29"></a>

##### [macro](ed5a) `40ants-mcp/tools:define-tool` name args &body body

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-4040ANTS-MCP-2FTRANSPORT-2FBASE-3FPACKAGE-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

### 40ANTS-MCP/TRANSPORT/BASE

<a id="x-28-23A-28-2825-29-20BASE-CHAR-20-2E-20-2240ANTS-MCP-2FTRANSPORT-2FBASE-22-29-20PACKAGE-29"></a>

#### [package](4df5) `40ants-mcp/transport/base`

<a id="x-2840ANTS-MCP-DOCS-2FINDEX-3A-3A-7C-4040ANTS-MCP-2FTRANSPORT-2FBASE-3FGenerics-SECTION-7C-2040ANTS-DOC-2FLOCATIVES-3ASECTION-29"></a>

#### Generics

<a id="x-2840ANTS-MCP-2FTRANSPORT-2FBASE-3ARECEIVE-MESSAGE-20GENERIC-FUNCTION-29"></a>

##### [generic-function](c339) `40ants-mcp/transport/base:receive-message` transport

Receive a `JSON-RPC` message, returns a message or `NIL`.

<a id="x-2840ANTS-MCP-2FTRANSPORT-2FBASE-3ASEND-MESSAGE-20GENERIC-FUNCTION-29"></a>

##### [generic-function](0897) `40ants-mcp/transport/base:send-message` transport message

Send a `JSON-RPC` message, returns no values.

<a id="x-2840ANTS-MCP-2FTRANSPORT-2FBASE-3ASTART-LOOP-20GENERIC-FUNCTION-29"></a>

##### [generic-function](efb3) `40ants-mcp/transport/base:start-loop` transport message-handler

Starts message processing using given transport.

<a id="x-2840ANTS-MCP-2FTRANSPORT-2FBASE-3ASTOP-LOOP-20GENERIC-FUNCTION-29"></a>

##### [generic-function](12ab) `40ants-mcp/transport/base:stop-loop` transport

Stops message processing using given transport.


[7318]: https://40ants.com/mcp/
[348e]: https://40ants.com/openrpc/
[e31f]: https://github.com/40ants/mcp
[04f0]: https://github.com/40ants/mcp/actions
[6eb7]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/content/base.lisp#L1
[41cb]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/content/base.lisp#L8
[46e2]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/content/base.lisp#L9
[e968]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/content/text.lisp#L1
[ba3a]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/content/text.lisp#L11
[eec6]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/content/text.lisp#L12
[bddb]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/http-transport.lisp#L1
[d2f4]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/http-transport.lisp#L35
[460e]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/http-transport.lisp#L36
[59d2]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/http-transport.lisp#L40
[c7d8]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/http-transport.lisp#L42
[5447]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/http-transport.lisp#L44
[e960]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/http-transport.lisp#L46
[e969]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/server/definition.lisp#L1
[2c74]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/server/definition.lisp#L104
[158a]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/server/definition.lisp#L38
[691e]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/server/definition.lisp#L39
[cd62]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/server/errors.lisp#L1
[22c6]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/server/errors.lisp#L10
[6786]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/stdio-transport.lisp#L1
[aaf4]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/stdio-transport.lisp#L19
[70f6]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/stdio-transport.lisp#L20
[ac77]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/stdio-transport.lisp#L24
[0bd0]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/stdio-transport.lisp#L28
[1aa4]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/tools.lisp#L1
[ed5a]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/tools.lisp#L10
[4df5]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/transport/base.lisp#L1
[efb3]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/transport/base.lisp#L10
[12ab]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/transport/base.lisp#L14
[c339]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/transport/base.lisp#L18
[0897]: https://github.com/40ants/mcp/blob/463da5a6e74072d182ab5e430b3bc45f28f7c80b/src/transport/base.lisp#L22
[6ed2]: https://github.com/40ants/mcp/issues
[473e]: https://modelcontextprotocol.io/
[8236]: https://quickdocs.org/alexandria
[3dbf]: https://quickdocs.org/bordeaux-threads
[49b9]: https://quickdocs.org/cl-ppcre
[482d]: https://quickdocs.org/clack
[b0a4]: https://quickdocs.org/clack-sse
[a9bd]: https://quickdocs.org/jsonrpc
[63d7]: https://quickdocs.org/lack
[6a02]: https://quickdocs.org/lack-request
[521b]: https://quickdocs.org/lack-response
[46a1]: https://quickdocs.org/local-time
[7f8b]: https://quickdocs.org/log4cl
[691c]: https://quickdocs.org/log4cl-extras
[b8fd]: https://quickdocs.org/openrpc-client
[c8e7]: https://quickdocs.org/openrpc-server
[626d]: https://quickdocs.org/sento
[c41d]: https://quickdocs.org/serapeum
[0027]: https://quickdocs.org/sse-server
[588d]: https://quickdocs.org/trivial-gray-streams
[d6b3]: https://quickdocs.org/uuid
[aba2]: https://quickdocs.org/yason

* * *
###### [generated by [40ANTS-DOC](https://40ants.com/doc/)]
