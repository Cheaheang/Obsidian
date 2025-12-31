HTTP Request headers

| Name            | Description                                                                                                              |
| --------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Accept          | Represent MIME type of response content to be accepted by the client                                                     |
| Accept-language | Represent the natural language of response content to be accepted by client                                              |
| Content-type    | MIME type of request body. **Ex:** text/ x-www-form-urlencoded, application/json, application/ xml, multipart/ form-data |
| Content-length  | length(byte) of request body                                                                                             |
| Date            | Date and time of request                                                                                                 |
| Host            | Server domain name                                                                                                       |
| User-agent      | Browser(client) details                                                                                                  |
| Cookie          | Contains cookies to send to server                                                                                       |
|                 |                                                                                                                          |
**You can get the header response data**
![[Pasted image 20251230115252.png]]

**HTTP Request Methods**

| Method | Description                                                                                         |
| ------ | --------------------------------------------------------------------------------------------------- |
| Get    | Request to retrieve information(page, entity, object or static file)                                |
| Post   | Sends an entity object to server; generally it will be inserted into the database                   |
| Update | Send s an entity object to server; generally updates all properties(full-update) it in the database |
| Patch  | Send s an entity object to server; generally updates few properties(full-update) it in the database |
| Delete | Request to delete an entity in the database                                                         |
There serveral way to paste data:
- Request.body
- "raw" request body
- QueryHelpers(For M
- icrosoft c#)