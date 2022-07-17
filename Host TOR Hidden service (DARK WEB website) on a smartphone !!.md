DARK WEB websites — TOR hidden services or the .onion sites which can only be accessed by connecting to TOR relay.

In this blog, We will host a TOR hidden service using termux in Android. This is incredibly cool, takes only a few minutes to setup…you’ve gotta try this!!

If you prefer a video tutorial, click [here](https://www.youtube.com/watch?v=oFUmVPjxRss)

If you don’t want to learn the process, I have my script to do the work for you. Check it here — https://github.com/sam5epi0l/onionX/

## Follow these steps -
* Get official termux app from F-DROID — https://f-droid.org/en/packages/com.termux/

* update packages ```pkg update -y```

* Install dependencies ```pkg i termux-api tor git -y```

* create torrc configuration 
```cp /data/data/com.termux/files/usr/etc/tor/torrc .
echo "HiddenServiceDir /data/data/com.termux/files/home/hidden_service/" >> torrc
echo "HiddenServicePort 80 127.0.0.1:1337" >> torrc```

* Start TOR hidden service```tor -f torrc```

* Check URL in hostname file ```cat /data/data/com.termux/files/home/hidden_service/hostname```

* (OPTIONAL) Start your Apache/nginx web server at port 1337

> I’m using python http server at 127.0.0.1:1337
```python3 -m http.server 1337```

Hurray! you finished all steps and the host is up and running DARK WEB website. Thank you so much for reading.
