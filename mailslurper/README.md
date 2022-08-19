# mailslurper smtp-server
> [MailSlurper](https://github.com/mailslurper/mailslurper) is a small SMTP mail server. SMTP server do not run over SSL by default

## Usage
### Building the image
```
docker build -t <name> .
```

### Running the image
```
docker run -it -p 2500:2500 -p 8080:8080 -p 8085:8085 --rm <name>
```

### MailSlurper Configuration
You can mount a local configuration file into the image if an alternative configuration file is needed
#### **`config.json`**
```
{
	"wwwAddress": "0.0.0.0",
	"wwwPort": 8080,
	"serviceAddress": "0.0.0.0",
	"servicePort": 8085,
	"smtpAddress": "0.0.0.0",
	"smtpPort": 2500,
	"dbEngine": "SQLite",
	"dbHost": "",
	"dbPort": 0,
	"dbDatabase": "./mailslurper.db",
	"dbUserName": "",
	"dbPassword": "",
	"maxWorkers": 1000,
	"autoStartBrowser": false,
	"keyFile": "",
	"certFile": "",
	"adminKeyFile": "",
	"adminCertFile": "",
	"authenticationScheme": "",
	"authSecret": "",
	"authSalt": "",
	"authTimeoutInMinutes": 120,
	"credentials": {}
}

```

#### **`docker-compose.yml`**
```
version: '3.7'
services:

    mailslurper:
        image: ajayy004/mailslurper
        ports:
            - "2500:2500"
            - "9000:8080"
            - "8085:8085"
        volumes:
            - $PWD/config.json:/opt/mailslurper/config.json
```
For an SSL-enabled SMTP server, you can obtain or generate certificates with the following commands
```
openssl genrsa -out mailslurper-key.pem 1024
openssl req -new -key mailslurper-key.pem -out mailslurper.csr
openssl x509 -req -in mailslurper.csr -signkey mailslurper-key.pem -out mailslurper-cert.pem
```
The certificate location should be updated in config.json and volume mounts should be added to docker-compose.yml once the certificates have been generated.

#### **`docker-compose.yml`**

```
version: '3.7'
services:

    mailslurper:
        image: ajayy004/mailslurper
        ports:
            - "2500:2500"
            - "9000:8080"
            - "8085:8085"
        volumes:
            - $PWD/config.json:/opt/mailslurper/config.json
            - $PWD/mailslurper-key.pem:/opt/mailslurper/mailslurper-key.pem
            - $PWD/mailslurper-cert.pem:/opt/mailslurper/mailslurper-cert.pem
```

#### **`config.json`**
```
{
	...,
	"keyFile": "./mailslurper-key.pem",
	"certFile": "./mailslurper-cert.pem",
	"adminKeyFile": "./mailslurper-key.pem",
	"adminCertFile": "./mailslurper-cert.pem",
    ...
}
```

See the [MailSlurper wiki](https://github.com/mailslurper/mailslurper/wiki) for more information on configuring MailSlurper.

### Using Docker Compose
There is an assumption you have installed docker-compose!

```
docker-compose up -d
```

