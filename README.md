gocsp-server
===========
This is a go implementation of a basic OCSP Responder.  
The two other options are:  
1. openssl ocsp - does not support GET (safari) and dies on a request it does not understand  
2. openca-ocspd - [has memory corruption bugs](https://github.com/openca/openca-ocspd/issues/17).  

It's a pretty simple protocol wrapped in HTTP.  

Refer to RFC 6960: https://tools.ietf.org/html/rfc6960

Building
--------
This was confirmed building with Go 1.20. Your milage may vary with other versions.  

1. Clone the repo  
2. cd into repo  
3. export GOPATH=$PWD (or just clone it into your GOPATH)  
4. go install gocsp-responder/main  

Features
--------
- Supports HTTP GET and POST requests  
- Meant to work with zca https://github.com/fizzywhizbang/zCA
- Nonce extension supported (will implement more if needed)  
- SSL support (not recommended)  

Limitations
-----------
- Only works with RSA keys (I think)
- Only PKCS1 (for keys) and PEM (for certs) supported.
  
Tests
-----
This has been tested and working with the `openssl ocsp` command, Chrome 109, Edge 110, Firefox 109, and Safari 16.3. It should still work for newer versions of these browsers.  

Options
-------
| Option   | Default Value                  | Description                                                                                                                 |
|----------|--------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| -bind    | ""                             | Bind address that the server will listen on (empty string is the same as 0.0.0.0 or all interfaces)                       |
| -cacert  | "ca.crt"                       | CA certificate filename                                                                                                    |
| -index   | "index.txt"                    | CA index filename (openssl 6 column index.txt file)                                                                        |
| -logfile | "/var/log/gocsp-responder.log" | File to log to                                                                                                             |
| -port    | 8888                           | Port that the server will listen on                                                                                        |
| -rcert   | "responder.crt"                | Responder certificate filename                                                                                             |
| -rkey    | "responder.key"                | Responder key filename                                                                                                     |
| -ssl     | false                          | Use SSL to serve. This is not widely supported and not recommended                                                         |
| -stdout  | false                          | Log to stdout and not the specified log file                                                                               |
| -strict  | false                          | Ensure Content-Type is application/ocsp-request in requests. Drop request if not. Some browsers (safari) don't supply this |

Notes
-----
The ocsp class is pretty much exactly copied from the golang.org/x/crypto/ocsp package. It had to be modified to support extensions so I just copied it in.  I may submit a change request for their ocsp class at some point but for now it is modified for this package and included. 
