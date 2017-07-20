[![License badge](https://img.shields.io/badge/license-Apache2-orange.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Documentation badge](https://readthedocs.org/projects/fiware-orion/badge/?version=latest)](http://doc-kurento.readthedocs.org/en/latest/)
[![Docker badge](https://img.shields.io/docker/pulls/fiware/orion.svg)](https://hub.docker.com/r/fiware/stream-oriented-kurento/)
[![Support badge]( https://img.shields.io/badge/support-sof-yellowgreen.svg)](http://stackoverflow.com/questions/tagged/kurento)

[![][KurentoImage]][Kurento]

Copyright © 2013-2016 [Kurento]. Licensed under [Apache 2.0 License].

# Install Kurento media server

Kurento Media Server (KMS) has to be installed on Ubuntu 14.04 LTS (64 bits).
In order to install the latest stable Kurento Media Server version (6.+) you have to type the following commands:

With trusty ubuntu version
```
echo "deb http://ubuntu.kurento.org trusty kms6" | sudo tee /etc/apt/sources.list.d/kurento.list
wget -O - http://ubuntu.kurento.org/kurento.gpg.key | sudo apt-key add -
sudo apt-get update
sudo apt-get install kurento-media-server-6.0
```

With xenial ubuntu version
```
echo "deb http://ubuntu.kurento.org xenial kms6" | sudo tee /etc/apt/sources.list.d/kurento.list
wget -O - http://ubuntu.kurento.org/kurento.gpg.key | sudo apt-key add -
sudo apt-get update
sudo apt-get install kurento-media-server-6.0
```

Disable ufw
`sudo ufw disable`

Instance trigger
Start
`sudo service kurento-media-server-6.0 start`
Stop
`sudo service kurento-media-server-6.0 stop`

#Install Kurento module
```
sudo apt-get install kms-pointerdetector-6.0
sudo apt-get install kms-chroma-6.0
sudo apt-get install kms-crowddetector-6.0
sudo apt-get install kms-platedetector-6.0
sudo service kurento-media-server restart
```

Install node
```
sudo apt-get install curl
curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
sudo apt-get install nodejs
sudo npm install -g n
sudo n lts
source ~/.bash_aliases
 
sudo npm install -g bower
```

Run Example
* WITH PROJECT EXIST server.js FILE
```
git clone https://github.com/nhancv/ot-kurento-node-webrtc.git 
cd to /webrtc_server/kurento/<demo_dir>
npm install
node server.js
```
Issue when install server-app on vps with root account, sometimes bower not work => frontend js not working completely.

Solved: cd to static folder and install bowser dependencies manually
`cd /webrtc_server/kurento/<demo_dir>/static && bower --allow-root install`

> These instructions work only if Kurento Media Server is up and running in the same machine as the tutorial. However, it is possible to connect to a remote KMS in other machine, simply adding the argument ws_uri to the npm execution command, as follows:
`npm start -- --ws_uri=ws://kms_host:kms_port/kurento`
In this case you need to use npm version 2. To update it you can use this command:
`sudo npm install npm -g`

`Default kms_port: 8888`


* WITH PROJECT WITHOUT server.js FILE
```
sudo npm install -g http-server
bower install
 ```
 
Then, in each demo folder execute this command:
`http-server -p 8443 -S -C keys/server.crt -K keys/server.key`
 
Finally, open https://localhost:8443/ in your browser to access to the demo.
 
Optional parameters
https://example.com/index.html?ws_uri=wss://example.org/kurento
 
 
All demos accept following parameters:
ws_uri: the WebSocket Kurento MediaServer endpoint. By default it connects to a Kurento MediaServer instance listening on the port 8433 on the same machine where it's being hosted the demo. The KMS must allow WSS (WebSocket Secure).
 
ice_servers: the TURN and STUN servers to use, formatted as a JSON string holding an array of RTCIceServerobjects (the same structure used when configuring a PeerConnection object), or an empty array to disabled them (this is faster and more reliable when doing tests on a local machine or LAN network). By default it use some random servers from a pre-defined list.
`https://example.com/index.html?ice_servers=[{"urls":"stun:stun1.example.net"},{"urls":"stun:stun2.example.net"}]
https://example.com/index.html?ice_servers=[{"urls":"turn:turn.example.org","username":"user","credential":"myPassword"}]
` 
Other parameters specific to each demo can be found defined at the top of their `index.js` file.
Securing Kurento Applications
First, you need to change the configuration file of Kurento Media Server, i.e.`/etc/kurento/kurento.conf.json`, uncommenting the following lines:
```
 "secure": {
	  "port": 8433,
	  "certificate": "defaultCertificate.pem",
	  "password": ""
	},
```

`sudo apt-get install gnutls-bin`
 
```bash

certtool --generate-privkey --outfile defaultCertificate.pem
echo 'organization = your organization name' > certtool.tmpl
certtool --generate-self-signed --load-privkey defaultCertificate.pem \
   --template certtool.tmpl >> defaultCertificate.pem

```
 
```
sudo cp defaultCertificate.pem /etc/kurento/
sudo chown kurento /etc/kurento/defaultCertificate.pem
sudo service kurento-media-server-6.0 restart
```

Go to https://localhost:8433 for self-sign
Install https://chrome.google.com/webstore/detail/web-socket-client/lifhekgaodigcpmnakfhaaaboididbdn to check connection


----------
[documentation]: http://www.kurento.org/documentation
[FIWARE]: http://www.fiware.org
[GitHub Kurento bugtracker]: https://github.com/Kurento/bugtracker/issues
[GitHub Kurento Group]: https://github.com/kurento
[kurentoms]: http://twitter.com/kurentoms
[Kurento]: http://kurento.org
[Kurento Blog]: http://www.kurento.org/blog
[Kurento FIWARE Catalog Entry]: http://catalogue.fiware.org/enablers/stream-oriented-kurento
[Kurento Netiquette Guidelines]: http://www.kurento.org/blog/kurento-netiquette-guidelines
[Kurento Public Mailing list]: https://groups.google.com/forum/#!forum/kurento
[KurentoImage]: https://secure.gravatar.com/avatar/21a2a12c56b2a91c8918d5779f1778bf?s=120
[Apache 2.0 License]: http://www.apache.org/licenses/LICENSE-2.0
[NUBOMEDIA]: http://www.nubomedia.eu
[StackOverflow]: http://stackoverflow.com/search?q=kurento
[Read-the-docs]: http://read-the-docs.readthedocs.org/
[readthedocs.org]: http://kurento.readthedocs.org/
[Open API specification]: http://kurento.github.io/doc-kurento/
[apiary.io]: http://docs.streamoriented.apiary.io/
[GitHub repository]: https://github.com/Kurento/kurento-tutorial-node
[kurento-client-js]: https://github.com/Kurento/kurento-client-js
[kurento-utils-js]: https://github.com/Kurento/kurento-utils-js
[Node.js]: http://nodejs.org/
