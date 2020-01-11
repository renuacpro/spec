# OONI Data Formats

| Authors    | Arturo Filastò et al. |
|------------|-----------------------|
| Version    | 0.3.2                 |
| Maintainer | Simone Basso          |

## Overview

The output of OONI _experiments_ (also known as _nettests_ or simply _tests_)
consists of a series of JSON documents separated by newline characters, also known
as [JSONL](http://jsonlines.org/). Every JSON document within the JSONL MUST be
a JSON object with a specific toplevel structure, also referred to as the _base data
format_. A _test template_ is a routine that performs functionality common across
several OONI experiments, e.g., fetching a web page using HTTP. Each test
template has its own data format.

Thus, the output of any experiment consists of the base data format, plus
the data format of zero or more test templates, plus zero of more fields
generated by the experiment itself. That is:

```JavaScript
{
    "data_format_version": "0.3.1",
    // other toplevel keys that are part of the base data format
    // ...
    "test_keys": {
        // keys written by test templates
        // ...
        // keys written by each experiment
        // ...
    }
}
```

Experiments MUST NOT use `test_keys` that conflict with the test keys
reserved by the test templates.

Keys starting with `x_` are always permitted anywhere. They are
experimental and should not be relied upon in the long term.

Consumers MUST be prepared for any field being `null` or missing but
producers SHOULD NOT omit fields (or emit `null`s) unless this has been
explicitly documented in the field description.

## Example

The following is a valid JSON that was edited for brevity.

```JSON
{
  "annotations": {
    "platform": "macos"
  },
  "data_format_version": "0.3.1",
  "measurement_start_time": "2020-01-10 17:25:19",
  "test_runtime": 4.426603178,
  "probe_asn": "AS30722",
  "probe_cc": "IT",
  "probe_ip": "127.0.0.1",
  "report_id": "20200110T172519Z_AS30722_5UdG13d6rEfOVCTHEdMjuXGah8vF6dpShA0jditnrHCmH10o1K",
  "resolver_asn": "AS15169",
  "resolver_ip": "172.217.34.2",
  "resolver_network_name": "Google LLC",
  "software_name": "miniooni",
  "software_version": "0.1.0-dev",
  "test_keys": {
    "agent": "redirect",
    "queries": [
      {
        "answers": [
          {
            "answer_type": "A",
            "ipv4": "149.154.167.99",
            "ttl": null
          }
        ],
        "engine": "system",
        "failure": null,
        "hostname": "web.telegram.org",
        "query_type": "A",
        "resolver_hostname": null,
        "resolver_port": null,
        "resolver_address": ""
      }
    ],
    "requests": [
      {
        "failure": null,
        "request": {
          "body": "",
          "body_is_truncated": false,
          "headers_list": [[
              "Host", "149.154.171.5"
            ], [
              "User-Agent", "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36"
            ], [
              "Content-Length", "0"
            ], [
              "Accept", "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8"
            ], [
              "Accept-Language", "en-US;q=0.8,en;q=0.5"
            ]
          ],
          "headers": {
            "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
            "Accept-Language": "en-US;q=0.8,en;q=0.5",
            "Content-Length": "0",
            "Host": "149.154.171.5",
            "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36"
          },
          "method": "POST",
          "tor": {
            "exit_ip": null,
            "exit_name": null,
            "is_tor": false
          },
          "url": "http://149.154.171.5/"
        },
        "response": {
          "body": "<html>\r\n<head><title>501 Not Implemented</title></head>\r\n<body bgcolor=\"white\">\r\n<center><h1>501 Not Implemented</h1></center>\r\n<hr><center>nginx/0.3.33</center>\r\n</body>\r\n</html>\r\n",
          "body_is_truncated": false,
          "code": 501,
          "headers_list": [[
              "Content-Length", "181"
            ], [
              "Server", "nginx/0.3.33"
            ], [
              "Date", "Fri, 10 Jan 2020 17:25:20 GMT"
            ], [
              "Content-Type", "text/html"
            ]
          ],
          "headers": {
            "Content-Length": "181",
            "Content-Type": "text/html",
            "Date": "Fri, 10 Jan 2020 17:25:20 GMT",
            "Server": "nginx/0.3.33"
          }
        }
      }
    ],
    "tcp_connect": [
      {
        "ip": "149.154.171.5",
        "port": 80,
        "status": {
          "failure": null,
          "success": true
        }
      }
    ],
    "telegram_http_blocking": false,
    "telegram_tcp_blocking": false,
    "telegram_web_failure": null,
    "telegram_web_status": "ok"
  },
  "test_name": "telegram",
  "test_start_time": "2020-01-10 17:25:19",
  "test_version": "0.0.4"
}
```

In this example:

- all toplevel keys belong to the base data format.

- the `agent` and `requests` keys within the `test_keys`
belong to the HTTP template data format.

- the `queries` key within the `test_keys` belongs to
the DNS template data format.

- the `tcp_connect` key within the `test_keys` belongs
to the TCPConnect data format.

- all the other keys within `test_keys` are generated
by the `telegram` experiment.

## Index

This directory contains the specification of:

- [the common data format](df-000-base.md)
- [the HTTP data format](df-001-httpt.md)
- [the DNS data format](df-002-dnst.md)
- [the Scapy data format](df-003-scapy.md)
- [the TCPTest data format](df-004-tcpt.md)
- [the TCPConnect data format](df-005-tcpconnect.md)
- [the TLSHandshake data format](df-006-tlshandshake.md)
- [the available failure strings](df-007-errors.md)
- [the network events data format](df-008-netevents.md)

See the [nettets](../nettests) directory for the experiments' specs.

## History

- `0.1.0` [2013-02-01]: original YAML format. New code MUST NOT use that.

- `0.2.0` [2016-01-27]: the new JSON format. OONI Probe CLI v2.x and OONI
Probe Mobile when using Measurement Kit as the measurement engine.

- `0.2.1` [2019-11-11]: should have been `0.3.0` because we moved the
`resolver_ip` field to toplevel keys. Was briefly used by unstable OONI
Probe CLI v3.0.0.

- `0.3.0` [2019-12-02]: same as `0.2.1` but renamed for correctness.

- `0.3.1` [2019-12-29]: added the `resolver_asn` and
`resolver_network_name` toplevel keys.

- `0.3.2` [2020-01-10]: explain why `test_start_time` may be problematic
and suggest using `measurement_start_time` instead. Since now the
`test_start_time` field is therefore marked as deprecated.