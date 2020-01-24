# All in one Tiny Tiny RSS solution Docker (over Tor) 
[Tiny Tiny RSS](https://github.com/HenryQW/Awesome-TTRSS) + [Mercury](https://www.github.com/HenryQW/mercury-parser-api) + [RSS Bridge](https://github.com/RSS-Bridge/rss-bridge) + [Politepol](https://github.com/taroved/pol) + [Tor](https://github.com/opsxcq/docker-tor)

## 1 - Download repository
``` shellsession
$ git clone https://github.com/marlluslustosa/ttrss-tor
$ cd ttrss-tor/
```


## 2 - Generating onion domain (TTRSS)

``` shellsession
$ docker run -it --rm --entrypoint shallot strm/tor-hiddenservice-nginx ^ttrs
--------------------------------------------------------------
Found matching domain after 5519 tries: ttrswuvo75a7aqm35.onion
--------------------------------------------------------------
-----BEGIN RSA PRIVATE KEY-----
MIICXQIBAAKBgQDSqBzjGxL+UFdrFJSdc+LJn3RrXiaZ7k6kgSw8KqOCSRgIr2qO
XZrCa3YHE+PqsfbDVF0GO0Xy3A9fsIxRFMUo3K++3BaVJslUbqK2TH9fJt5Ji1b6
N5UzXsEzf73atXwMF63hgVFZFLhfSWH8jGE1svwDXn0YQWP88PVX34SrWQIDASsd
AoGAUWdd+/m9TrTQyqK0IbzIr0fYQ5gDq4mv1GLEYjR4SWF8pSCxL1yOBsmQ02sj
BSS2Vw4dpFfloCrRw2ipM8ac4kdLGCoYefQHwW2Kfdf9raVfPDP7vcxrs37sOgOh
2rSXCOOrmcoMrEka2/OTGW15jaNUEEoWacS3YL1Fj0Bi6g0CQQD4ZmBiF6qu2XnT
8lMr1Asdz3K8fYiyfl6CzHItUubAbQ8ipv12q8CerJqk3dO98V+w8llAsQ7BT5wq
8AZOPQR3AkEA2RobnACDvb2Jw+dYSFsqrHyIDojKsrNiDEFedkiFijRFqme+nrif
kJ4yTnSiphC+rSSBbvYMawsqiWBA7UPSrwJBAKXSVQClxNUpJ2PZt91HZAtuipRt
t8suGIY4mot1iDRN0XdiNN8TNZ3qLag7wUU4or+Yn/3Xae1euHpyftTxmYsCQQCd
oJxsGotYx62ULxPqz0um7yEWOU6hUAy8MB3X3FcTCjGO0PPKpfJ2ntXo0Ajcp5ci
msi81/e9DTnF9mPjtsY9AkAUG6heBlETMFzyka9FHPgu9aN2kRwvJ3QZDHuPxYG4
VZwljLxstlx57+N74D0aj6wrJw+iBH2BI+b+ZpnLXyy7
-----END RSA PRIVATE KEY-----
```

## 2.1 - Generating onion domain (Politepol)

``` shellsession
$ docker run -it --rm --entrypoint shallot strm/tor-hiddenservice-nginx ^poli
--------------------------------------------------------------
Found matching domain after 5519 tries: poliwuvo75a7aqm35.onion
--------------------------------------------------------------
-----BEGIN RSA PRIVATE KEY-----
MoJxsGotYx62ULxPqz0um7yEWOU6hUAy8MB3X3FcTCjGO0PPKpfJ2ntXo0Ajcp5c
XZrCa3YHE+PqsfbDVF0GO0Xy3A9fsIxRFMUo3K++3BaVJslUbqK2TH9fJt5Ji1b6
N5UzXsEzf73atXwMF63hgVFZFLhfSWH8jGE1svwDXn0YQWP88PVX34SrWQIDASsd
AoGAUWdd+/m9TrTQyqK0IbzIr0fYQ5gDq4mv1GLEYjR4SWF8pSCxL1yOBsmQ02sj
BSS2Vw4dpFfloCrRw2ipM8ac4kdLGCoYefQHwW2Kfdf9raVfPDP7vcxrs37sOgOh
2rSXCOOrmcoMrEka2/OTGW15jaNUEEoWacS3YL1Fj0Bi6g0CQQD4ZmBiF6qu2XnT
8lMr1Asdz3K8fYiyfl6CzHItUubAbQ8ipv12q8CerJqk3dO98V+w8llAsQ7BT5wq
8AZOPQR3AkEA2RobnACDvb2Jw+dYSFsqrHyIDojKsrNiDEFedkiFijRFqme+nrif
AoGAUWdd+/m9TrTQyqK0IbzIr0fYQ5gDq4mv1GLEYjR4SWF8pSCxL1yOBsmQ02sj
t8suGIY4mot1iDRN0XdiNN8TNZ3qLag7wUU4or+Yn/3Xae1euHpyftTxmYsCQQCd
oJxsGotYx62ULxPqz0um7yEWOU6hUAy8MB3X3FcTCjGO0PPKpfJ2ntXo0Ajcp5ci
N5UzXsEzf73atXwMF63hgVFZFLhfSWH8jGE1svwDXn0YQWP88PVX34SrWQIDASsd
VZwljLxstlx57+N74D0aj6wrJw+iBH2BI+b+ZpnLXyy7
-----END RSA PRIVATE KEY-----
```

## 3 - Editing `docker-compose` file. Edit the environment variable "SELF_URL_PATH" for the onion domain generated in the previous step (2 - Generating onion domain (TTRSS)).

```yml
version: "3"

  service.rss:
    image: wangqiru/ttrss:latest
    container_name: ttrss
    expose:
      - 80
    environment:
      - SELF_URL_PATH=http://ttrswuvo75a7aqm35.onion # please change to your own domain
      - DB_HOST=database.postgres
      - DB_PORT=5432
      - DB_NAME=ttrss
      - DB_USER=postgres
      - DB_PASS=ttrss # please change the password
      - ENABLE_PLUGINS=auth_internal,fever # auth_internal is required. Plugins enabled here will be enabled for all users as system plugins
    stdin_open: true
    tty: true
    restart: always
    networks:
      - ttrss-network
    command: sh -c 'sh /wait-for.sh $$DB_HOST:$$DB_PORT -- php /configure-db.php && exec s6-svscan /etc/s6/'
```
## 3.1 Editing `docker-compose` file. Edit the "PRIVATE_KEY" environment variable for the private keys generated in the previous step 2.

```yml
  ttrsstor:
    image: strm/tor
    container_name: ttrsstor
    restart: always
    depends_on:
      - service.rss
    networks:
      - ttrss-network
    environment:
      LISTEN_PORT: "80"
      REDIRECT: "ttrss:80"
      PRIVATE_KEY: |
        -----BEGIN RSA PRIVATE KEY-----
        MIICXQIBAAKBgQDzBAiMOwcVGYcGzRTOf/YQhnFO2xjwlbAJ1JrZYiKFVcIgTUWb
        hscKezpbQMNG0K/ljuuD98OvuxdzMsO5NSjyu98uypYvfGddYkvib1Rk7eyj2EzJ
        1BIw4pU9d1asX566uzIFW8FMOTzinbp1Z/oHfyLwujZ/88PhZ8qZr6o+0QIDAQAB
        AoGABt4KT8wrOxFpm2uYNu2uynDCKvROFB5mxyBW7+WyDAqMXdVRLj/0x/sLfyCp
        ZArprZcIWMZbpU+oLf01QrqZ2ZQUtcvpBHE1TNLGlI9KU2kmqTzpl658R1nqFyCb
        AJWa28WsIrfcd5ebEaRFxUROzTmXwbRlXFzFRh/XmOfg9kECQQD5qgeYOzR2aTdw
        BSS2Vw4dpFfloCrRw2ipM8ac4kdLGCoYefQHwW2Kfdf9raVfPDP7vcxrs37sOgOh
        wh8DRSEFAkEA+S7PwBDXVEQWc96ojSTHllfbvMuerPJ/W3lz0yghhoaN72Y6W40X
        Pk81eo6eRTlPjUCvWesorFZ8RltQK9xAXQJBALkmaT9yGLP/z4dqMoxH7Esr7Nkm
        qDIBm1/1m9kAZuMfuQ2B+ds/wVqMRB4cYI5BiN0dQIkEYdRYc/py/cDVEWkCQQCy
        N5UzXsEzf73atXwMF63hgVFZFLhfSWH8jGE1svwDXn0YQWP88PVX34SrWQIDASsd
        aMmI2eC4z/Ga+R9ZoTJdAkAo4rC2NtT0uD1On4Qf9w97ykr/o6JPBQPTpWSkq2NR
        8rDZe3IwO47HsFpWYHgnQCrT3Gy6z3lC6z7u20Ib63jl
        -----END RSA PRIVATE KEY-----

 politepoltor:
    image: strm/tor
    container_name: politepoltor
    restart: always
    depends_on:
      - politepol
    networks:
      - ttrss-network
    environment:
      LISTEN_PORT: "8088"
      REDIRECT: "politepol:8088"
      PRIVATE_KEY: |
        -----BEGIN RSA PRIVATE KEY-----
        MIICXAIBAAKBgQDwJNv4M3RfERvwphBfH0ncoIxaBFniEuaMrwlVCAVEtwDvlC7d
        dW266oxMUYOVXPg0Qu8D1iFUAMpTsR/dH3o5/c3Sat24qIQ47bFseLhf48/dURXn
        itmq2+qbffmc8ainXIvwzEdJuVd9gUoB5GGU6xXqppBMr/0m1W2YqxUtLQIDAQFv
        AoGABJcks9TRHB9/ScK3CqfPpQnV8POJxspAj3Uha0NolP9JkGTwvIALCuPu7F+w
        8JlUblj60BrKjh9f6SIe8i8uArdw7kUl7edvz9tdv0GZkPeZrwmaD+t8XuwiRK2Z
        fVyZ0QIjSMNqZfbLhj1MHyDRfRPdTsLPbU34JTaS1Vy3D78CQQD9S0xMPXxap12u
        kJ4yTnSiphC+rSSBbvYMawsqiWBA7UPSrwJBAKXSVQClxNUpJ2PZt91HZAtuipRt
        FknIiqRRAkEA8rWZOcs1pmTG2kewO5Ek0f0UOMUl97AaZBQLQGpIZWO1cTKX0lPx
        mglpyvvh5BY4wYXccobbhqbWjTKN6gOQHQJAb0Z1ugiXVrcPm6TSZzVbrVBq2fxR
        D6rvgtLQ+9YsMFnqeVVA7ocMJq5O1U+oUPAGct37rTdQ7+WrSXshI+1+DwJAcgNR
        5SN+L/kotdPehcGpkJOXLa6zoeG/dfhQ7KR0+yD7dz8A4Xml5fzoxc023UVA+1Je
        st9CN0gjQxnFk2LwBwJBANPcRHHU1+6oeeJVzBcyL4tddCZ/7N3rR6/S4XePFUbA
        VZwljLxstlx57+N74D0aj6wrJw+iBH2BI+b+ZpnLXyy7=
        -----END RSA PRIVATE KEY-----
```

## 4 - Create whitelist.txt file (RSS Bridge requisite) on folder. Put * if you want to enable all repositories.
```shellsession
$ echo "*" > whitelist.txt
```

## 5 - Additional Information 1
By default, the ttrss database will be created inside the "./postgres/" (postgres container) folder and the politepol database will be written to "./mysql/" (dbpolitepol container).

# Building
```shellsession
$ docker-compose up -d
```
## Access TTRSS 
In the [Tor browser](https://www.torproject.org/download/), enter http://ttrswuvo75a7aqm35.onion (Domain Generated on step 2)

## Using politepol
In the [Tor browser](https://www.torproject.org/download/), enter http://poliwuvo75a7aqm35.onion:8088 (Domain Generated on step 2.1)

### Additional Information 2
- When adding a feed for rssbridge, just put "http://rssbridge/..." in the domain name (ex: http://rssbridge/?action=display&bridge=Instagram&u=fortaleza&media_type=all&format=Atom).
- Likewise, when adding a feed related to politepol in the TTRSS, change the .onion domain to "http://politepol:8088/..." (ex: http://politepol:8088/feed/24)
