## BEST Method: Install Ubuntu Desktop ( Similar to Windows on VPS)

منبعش کراپیتون هستش و بنظر نصبش مشکل امنیتی نداره. ولی بازم ولت اصلی متامسک رو داخلش اینپورت نکن!! چه ولت W00 و چه ولت های P01 تا P05 رو داخل هیچ سروری وارد نکن!!!

**** حتما اوبونتو 22 نیازه ****
 بعد نصب اوبونتو
روی سرور linux مجازی نصب کردم با کامند طبق ویدیو:

https://drive.google.com/file/d/1NFk-Rxj8GlP2Cp6c7JT90uPO8_EiyVWd/view?usp=sharing
```
sudo apt update
sudo apt install curl
curl -O https://gist.githubusercontent.com/NodeFarmer/a533a2e5e7ae8174e06d8e8830721ab6/raw/UbuntuDesktop.sh && chmod +x UbuntuDesktop.sh && ./UbuntuDesktop.sh

```
بعد از نصب، از طریق ریموت دسکتاپ کامپیوتر می شه به لینوکس نصب شده وارد شد ولی گاهی کروم داخل لینوکس درست ران نمی شه، در این حالت باید مرحله پایین، یعنی نصب کروم روی لینوکس را مجدد انجام داد:


## 2. Install Google Chrome on the VPS
**1. Open Terminal (if it hasn't automatically opened in the VNC session).**

**2.Download Google Chrome:**
```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

**3. Install Google Chrome:**
```bash
sudo apt install ./google-chrome-stable_current_amd64.deb
```

If you encounter dependency errors, run:
```bash
sudo apt --fix-broken install
```

**4. Launch Chrome in VNC (add --no-sandbox if necessary):**
```bash
google-chrome --no-sandbox
```




------------------------------------------------------------------------------------------------------------------------
این یکی روش معین(توییتر) هستش که چند ماه خودم ران کردم ولی یه مقدار سرعت سرور پایین هستش و نه تنها منابع خیلی زیادی میخواد بازم کروم داخلش قطع و وصل میشه:

و تنطیماتش باید دستی انجام بشه ولی بجاش بیشترین امنیت رو داره:

# Install Chromium Linux Browser
Chromium is an open-source browser project that aims to build a safer, faster, and more stable build by Google
* You can easily access a browser in your non-gui Linux server
* You can easily run your Node Extensions
* 
این یکی روش معین(توییتر) هستش که چند ماه خودم ران کردم ولی یه مقدار سرعت سرور پایین هستش و نه تنها منابع خیلی زیادی میخواد بازم کروم داخلش قطع و وصل میشه:

و تنطیماتش باید دستی انجام بشه ولی بجاش بیشترین امنیت رو داره:
## Install Docker
```console
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Docker version check
docker --version
```

## Timezone check
```
realpath --relative-to /usr/share/zoneinfo /etc/localtime
```

## Install Chromium
**1. Create directory**
```
mkdir chromium
cd chromium
```

**2. Create `docker-compose.yaml` file**
```
nano docker-compose.yaml
```

**3. Paste the following code in it**
* `CUSTOM_USER` & `PASSWORD`: Replace your favorite credentials to login to chromium
* `TZ`: Replace with your server timezone
* `CHROME_CLI`: The main page when you open browser
* `ports`: You can replace `3010` & `3011` if they have conflict
```
---
services:
  chromium:
    image: lscr.io/linuxserver/chromium:latest
    container_name: chromium
    security_opt:
      - seccomp:unconfined #optional
    environment:
      - CUSTOM_USER=     #Replace username
      - PASSWORD=    #Replace password
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - CHROME_CLI=https://github.com/0xmoei #optional
    volumes:
      - /root/chromium/config:/config
    ports:
      - 3010:3000   #Change 3010 to your favorite port if needed
      - 3011:3001   #Change 3011 to your favorite port if needed
    shm_size: "1gb"
    restart: unless-stopped
```
> To save and exit: `Ctrl+X+Y+Enter` 

## Run Chromium
```console
cd $HOME && cd chromium

docker compose up -d
```
**The application can be accessed by going to one of these addresses in your local PC browser**
* http://Server_IP:3010/
* https://Server_IP:3011/

## Optional: Edit VPS RAM for Chromium
اگه کرومیوم رو برای نودهای اکستنشن ( grass, nodepay,... ) نصب کردید و کنده و براتون لود نمیشه تغییرات زیر رو انجام بدید تا کرومیوم بتونه از رم بیشتری استفاده کنه و سرعتش بیشتر شه.
```
docker stop chromium
```
```
docker rm chromium
```
```

cd chromium
```
```

nano docker-compose.yaml
```
حالا قسمت زیر رو باید تغییر بدید.
بستگی به رم سرورتون داره این عدد
اگه رم سرور مثلا 16 گیگه می تونید تا 10 گیگ رو اجازه بدید که کرومیوم ازش استفاده کنه.

--shm-size="10g"


و بعد با دستور زیر دوباره کرومیوم رو ران کنید.
```

docker compose up -d

```
## Option 2: Stop and Delete Chromium
```
docker stop chromium
docker rm chromium
docker system prune
```


# Install Proxy on Chromium
## 1) Buy Proxy
* You can use any reliable platform to buy a **Static Residential** proxy.
* I bought a proxy via crypto payments on [iproyal](https://iproyal.com/?r=835672) or [webshare]([https://iproyal.com/?r=835672](https://www.webshare.io/?referral_code=la7tdfj39ss5))

## 2) Install Proxy in Docker
**1- Stop currently running container**
```bash
docker compose down -v
```

**2- Update your `docker-compose.yml`:**
* Replace `CUSTOM_USER` & `PASSWORD` with your Chromium credentials.
* Delete one of `CHROME_CLI` lines depending your proxy is `http` or `socks5` and replace `proxy.example.com:1080` with your proxy address and port.
```yaml
---
services:
  chromium:
    image: lscr.io/linuxserver/chromium:latest
    container_name: chromium
    security_opt:
      - seccomp:unconfined
    environment:
      - CUSTOM_USER=      #Replace username
      - PASSWORD=      #Replace username
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Berlin
      - CHROME_CLI=--proxy-server=http://proxy.example.com:1080 https://google.com
      - CHROME_CLI=--proxy-server=socks5://proxy.example.com:1080 https://google.com
    volumes:
      - /root/chromium/config:/config
    ports:
      - 3010:3000
      - 3011:3001
    shm_size: "1gb"
    restart: unless-stopped
```

**3- Start container**
```bash
docker compose down -v
docker compose up -d
```

**4- Access to your Chromium using `http://Server_IP:3010/` or `https://Server_IP:3011/`**
* First it asks you to enter your `chromium` credential, then it asks for `proxy` credential (if your proxy has credential).

![image](https://github.com/user-attachments/assets/50a05730-b4c3-45cd-967a-f3a8e156e22d)



-----------------------------------------------------------------------------------------------------------------------

## BEST Method: Install Ubuntu Desktop ( Similar to Windows on VPS)

منبعش کراپیتون هستش و نصبش ممکنه مشکل امنیتی نداره. ولی بازم ولت اصلی متامسک رو داخلش اینپورت نکن!! چه ولت W00 و چه ولت های P01 تا P05 رو داخل هیچ سروری وارد نکم!!!

**** حتما اوبونتو 22 نیازه ****
 بعد نصب اوبونتو
روی سرور linux مجازی نصب کردم با کامند طبق ویدیو:

https://drive.google.com/file/d/1NFk-Rxj8GlP2Cp6c7JT90uPO8_EiyVWd/view?usp=sharing
```
sudo apt update
sudo apt install curl
curl -O https://gist.githubusercontent.com/NodeFarmer/a533a2e5e7ae8174e06d8e8830721ab6/raw/UbuntuDesktop.sh && chmod +x UbuntuDesktop.sh && ./UbuntuDesktop.sh

```

## Option 3: Ubuntu-Installation in Windows (WSL)

https://cryptonodehindi.medium.com/ubuntu-installation-in-windows-wsl-fce2d18fea8c

منبعش هندیه و زیاد قابل اعتماد نیست :(


To install WSL with the default settings, enter:
```
wsl --install
```
آموزش ویدیویی:
https://www.youtube.com/watch?v=HrAsmXy1-78

