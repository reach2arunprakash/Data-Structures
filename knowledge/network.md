# Network

1. What happens when you type in a URL in browser
Ans :

    1. browser checks cache; if requested object is in cache and is fresh, skip to #9
    2. browser asks OS for server's IP address
    3. OS makes a DNS lookup and replies the IP address to the browser
    4. browser opens a TCP connection to server (this step is much more complex with HTTPS)
    5. browser sends the HTTP request through TCP connection
    6. browser receives HTTP response and may close the TCP connection, or reuse it for another request
    7. browser checks if the response is a redirect or a conditional response (3xx result status codes), authorization request (401), error (4xx and 5xx), etc.; these are handled differently from normal responses (2xx)
    8. if cacheable, response is stored in cache
    9. browser decodes response (e.g. if it's gzipped)
    10. browser determines what to do with response (e.g. is it a HTML page, is it an image, is it a sound clip?)
    11. browser renders response, or offers a download dialog for unrecognized types
    
    
---
###Reference
1. http://stackoverflow.com/questions/2092527/what-happens-when-you-type-in-a-url-in-browser