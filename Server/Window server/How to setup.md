 
## Install window server


## Clone project from source(Gitlab, Github)
- Generate ssh key 
- Put to the source 
- Clone your project to **C:\inetpub\wwwroot**
```
git clone 
```
- Config environment

## Give Permission to your project
- Go to project directory and right click to select properties
- Then go to Security and select Advanced
- Disable inheritance and select Principal for IIS_IUSRS
- Tick write permission, then save and Enable inheritance back
## Config to make it public
- To firewall, then **Advanced security** and select **Inbound Rules**, then create **New Rule....**
- **Rule port :** Select Port
- Select **Allow the connection**
- Input name and description
