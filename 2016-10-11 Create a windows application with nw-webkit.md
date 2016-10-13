Create a windows application with nw-webkit 


# Download nw from nwjs.io

we use with without sdk file. The sdk package only contains files:

![](http://newimg.globalmarket.com/group0/d6/6f/19d1dfc76be442d2e3a573981e1e.png)

- chromedriver.exe ---?
- nacl_irt_x86_64.exe ---?
- nwjc.exe ---?
- payload.exe --- ?

unzip the file to a folder `c:/Desktop/mvo-web`


# Create a file : `package.json` in the folder:

samples 

```
{
  "main": "http://www.xuyubang.com/admin",
  "name": "nw-demo",
  "description": "demo app of NW.js",
  "version": "0.1.0",
  "keywords": [ "demo", "NW.js" ],
  "window": {
    "title": "NW.js demo",
    "icon": "link.png",
    "toolbar": false,
    "frame": true,
    "width": 1200,
    "height": 800,
    "position": "center",
    "min_width": 400,
    "min_height": 200
  },
  "webkit": {
    "plugin": true
  },
  "dependencies": {
    "deck":"latest"
  }
}
```

# Install reshacker_setup.exe, Change the nw.exe icons to gmc logo 

import gmc_logo.res file is ok.

![](http://newimg.globalmarket.com/group0/9f/2f/bb7555abb1fa8844a62c0ed455dd.png)

# Install nisedit2.0.3.exe & nsis-3.0-setup.exe

nisedit --- edit the nsis config file 
nsis    --- create Setup.exe by the nsis config file

> Use nisedit wizard to create the config file:

![](http://newimg.globalmarket.com/group0/ca/4e/d0f9c4b72226fe5d120d0099baff.png)
![](http://newimg.globalmarket.com/group0/48/a8/971599cf0ca9b7de5df2171ca9cd.png)
![](http://newimg.globalmarket.com/group0/39/70/268b5bc2094514d570a9ed24563a.png)
![](http://newimg.globalmarket.com/group0/a8/70/f6331f2521e0837d31446c6b1041.png)
![](http://newimg.globalmarket.com/group0/ec/fa/1d48c40b4a6623d558afdbdbbe97.png)

> Use nsis to complile the config file:

![](http://newimg.globalmarket.com/group0/91/6b/826235b63604acd74c1d8bc2c07b.png)
![](http://newimg.globalmarket.com/group0/15/d9/c4030a440e29852cd70f303ae3a7.png)
