
| Overall Code | Overall Info  | Specific Code | Desc                  |
| ------------ | ------------- | ------------- | --------------------- |
| 1xx          | Informational | 101           | Switch Protocol       |
| 2xx          | Successs      | 200           | OK                    |
| 3xx          | Redirection   | 302           | Found                 |
|              |               | 304           | Not Modified          |
| 4xx          | Client error  | 400           | Bad Request           |
|              |               | 401           | Unathorized           |
|              |               | 404           | Not found             |
| 5xx          | Server        | 500           | Internal server error |
|              |               |               |                       |
**HTTP Response Headers**

| Name                        | Description                                                                                        |
| --------------------------- | -------------------------------------------------------------------------------------------------- |
| Date                        | Date and time of the response                                                                      |
| Server                      | Name of the server                                                                                 |
| Content-type                | MIME type of response body. **ex:** text/plain, text/html , application/json, application/xml, etc |
| Content-length              | length(byte) of response body                                                                      |
| Cache-Control               | Indicate number of second that response can be cached at the browser                               |
| Set-cookie                  | Contains cookies to send to browser                                                                |
| Access-Control-Allow-Origin | Used to enable CORS( Cross-Origin-Resource-Sharing)                                                |
| Location                    | Contains url to redirect                                                                           |

