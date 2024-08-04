# Http Crash Course 

## Terms
- ### URx
  - URI: Uniform Resource Indentifier
  - URL: Uniform Resource Locator
  - URN: Uniform Resource Name 
All these are technical jargons ; they have the same meaning that is address/location of that site

- ### http
   - http Methods and headers
   - Http: Hyper Text Transfer Protocol
   - inexpensive way of communication or transfering data
   - http vs https : both work on the same principle
     - https add one more layer to it : the data in the route is encoded (not readable) 
   - Conecpt of O.S , Network and cryptography

## http headers
when http request is sent some info is also sent along with file and all that is Meta data :-
  - eg Like in file Meta data :name , size , createdby , modify
  - Meta data is a key value pair
  - Use case 
     - caching 
     - authentication : sesion / refresh token 
     - manage state : eg guest / loggedin to access cart in ecommerce website
  - (x - prefix) before 2012 convention


### Types of http headers 
No standard category , can vary on the book or tutor
1. Request Headers: from client
2. Response Headers: from server
3. Representation Headers: encoding / compression 
   - as there is Network limit : so we have to ensure no lag , so encode
   - Mobile app , zip , graph chart 
   - razorpay : security
4. Payload Headers: data 
  - id , email


### Commom Header
1. Accept :  what data to be accepted ; most common application form or json or html/text
2. User Agent : request from which website knows it is running on  which browser , its engine support , 
  - you may get a pop up : use our app for better experience 
    - it is this header that tells the website that it is running or browser 
3. Authorization : frontend 
  - bearer keyword 
  - token: JWT 
4. Content Type: what type png , pdf
5. Cookie: time , login details
6. Cache: control data expiry

## CORS 
1. Access - Control - Allow - Origin
2. Access - Control - Allow - Credentials
3. Access - Control - Allow - Method

## Security
1. Cross-Origin-Embedded Policy
2. Cross-Origin Opener Policy
3. Content Security Policy
4. X -XSS - Protection 

## HTTP Methods
Basic set of operations that can be used to interact with server  ,
- Data to be written in a Db
- Data to be fetched (read)
- update whole or some portion 

1. GET: retrieve a resource
    - eg; give me this user details 
2. HEAD : No message body(response headers only) ; cache control 
3. OPTIONS : what operations are available ; asked by the user from server
4. TRACE:  loopback test ; get some data ; debugging; resource is behind proxy ; timely happening 
5. PUT : replace a resource
6. PATCH : change part of a resource
7. POST : interact with a resource (mostly add) 
    - add new user , add new category

## Tools
- Thunder Client : vscode extension
- Postman

## HTTP STATUS CODE

1. 1xx :- Informational
1. 2xx :- Success
1. 3xx :- Redirection
   - temporarily to other resource
1. 4xx :- Client Error
   - login token not given
   - image oversize
1. 5xx :-Server Error
    - Network break 
    - congestion ; retry 

xx: range from 00 to 99

### Common
- 100 : continue
- 102: Proccessing
   - wait 3sec ; if task pending
- 200 : OK
- 201 : resouce created
- 307 : temporary redirect
- 308 : Permanent redirect
- 400 : Bad Request
- 401 : unauthorized
- 402 : Payment required
- 404 : Not Found
- 500 : Internal Sever
- 504 : Gateway Time Out 