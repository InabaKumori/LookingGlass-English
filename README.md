# ALS - Another Looking-glass Server

<img width="1604" alt="Screenshot 2023-09-06 at 10 10 32 AM" src="https://github.com/InabaKumori/LookingGlass-English/assets/51642034/f3eb0083-7ec0-4e58-8460-0358c665e167">


## Quick start

```
docker run -d --name looking-glass --restart always --network host trcoot/looking-glass
```
## Starting the Looking-glass server with port 5001 with iperf3 & fakeshell disabled
```
docker run -d --name looking-glass --restart always --network host -e HTTP_PORT=5001 -e UTILITIES_IPERF3=false -e UTILITIES_FAKESHELL=false trcoot/looking-glass
```


[DEMO](http://163.123.192.89:5001/)

## Host Requirements
 - Can run docker (yes, only docker is required)

## Image Environment Variables
| Key                       | Example                                                                | Default                                                    | Description                                                                             |
| ------------------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| LISTEN_IP                 | 127.0.0.1                                                              | (all ip)                                                   | which IP address will be listen use                                                     |
| HTTP_PORT                 | 80                                                                     | 80                                                         | which HTTP port should use                                                              |
| SPEEDTEST_FILE_LIST       | 100MB 1GB                                                              | 1MB 10MB 100MB 1GB                                         | size of static test files, separate with space                                          |
| LOCATION                  | "this is location"                                                     | (from maxmind database, ip via PUBLIC_IPV4 or PUBLIC_IPV6) | location string                                                                         |
| MAXMIND_KEY               | THE_KEY                                                                | (empty)                                                    | about more https://dev.maxmind.com/geoip/geolite2-free-geolocation-data                 |
| PUBLIC_IPV4               | 1.1.1.1                                                                | (fetch from http://ifconfig.co)                            | The IPv4 address of the server                                                          |
| PUBLIC_IPV6               | fe80::1                                                                | (fetch from http://ifconfig.co)                            | The IPv6 address of the server                                                          |
| DISPLAY_TRAFFIC           | true                                                                   | true                                                       | Toggle the streaming traffic graph                                                      |
| ENABLE_SPEEDTEST          | true                                                                   | true                                                       | Toggle the speedtest feature                                                            |
| UTILITIES_PING            | true                                                                   | true                                                       | Toggle the ping feature                                                                 |
| UTILITIES_SPEEDTESTDOTNET | true                                                                   | true                                                       | Toggle the speedtest.net feature                                                        |
| UTILITIES_FAKESHELL       | true                                                                   | true                                                       | Toggle the HTML Shell feature                                                           |
| UTILITIES_IPERF3          | true                                                                   | true                                                       | Toggle the iperf3 feature                                                               |
| UTILITIES_IPERF3_PORT_MIN | 30000                                                                  | 30000                                                      | iperf3 listen port range - from                                                         |
| UTILITIES_IPERF3_PORT_MAX | 31000                                                                  | 31000                                                      | iperf3 listen port range - to                                                           |
| SPONSOR_MESSAGE           | "Test message" or "/tmp/als_readme.md" or "http://some_host/114514.md" | ''                                                         | Show server sponsor message (support markdown file, required mapping file to container) |


## Features
- [x] HTML 5 Speed Test
- [x] Ping - IPv4
- [x] iPerf3 server
- [x] Streaming traffic graph
- [x] Speedtest.net Client
- [x] Online shell box (limited commands)

## Thanks to
https://github.com/librespeed/speedtest

https://www.jetbrains.com/

## License

Code is licensed under MIT Public License.

* If you wish to support my efforts, keep the "Powered by LookingGlass" link intact.
