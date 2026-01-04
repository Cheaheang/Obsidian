 
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

## Create tunnel from Cloudflared
### Download cloudflared

On your **Windows Server**:
https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/installation

Download **cloudflared-windows-amd64.exe**

Rename it to :
```
cloudflared.exe
```
Place it in :
```
C:\cloudflared\
```
Go to that directory and login
```
cd \
cloudflared.exe login
cloudflared.exe tunnel create cheaheang-tunnel
```
Create folder and file.yml
```
mkdir cloudflared
cd cloudflared
echo hello word > config.yml
```
Go to that file and remove the text and paste this text:
```
tunnel: cheaheang-tunnel
credentials-file: C:\Users\Adminstrator\.cloudflared\<TUNNEL-ID>.json

ingress:
  - hostname: cheaheang.dev
    service: http://localhost:80
  - hostname: www.cheaheang.dev
    service: http://localhost:80
  - service: http_status:404

```
Then run this cmd
```
cloudflared.exe tunnel --config C:\cloudflared\config.yml run cheaheang-tunnel
```