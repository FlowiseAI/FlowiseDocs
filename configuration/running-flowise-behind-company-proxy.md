# Running Flowise behind company proxy

If you're running Flowise in an environment that requires a proxy, such as within an organizational network, you can configure Flowise to route all its backend requests through a proxy of your choice. This feature is powered by the `global-agent` package.

[https://github.com/gajus/global-agent](https://github.com/gajus/global-agent)

## Configuration

There are 2 environment variables you will need to run Flowise behind a company proxy:

| Variable                   | Purpose                                                                          | Required |
| -------------------------- | -------------------------------------------------------------------------------- | -------- |
| `GLOBAL_AGENT_HTTP_PROXY`  | Where to proxy all server HTTP requests through                                  | Yes      |
| `GLOBAL_AGENT_HTTPS_PROXY` | Where to proxy all server HTTPS requests through                                 | No       |
| `GLOBAL_AGENT_NO_PROXY`    | A pattern of URLs that should be excluded from proxying. Eg. `*.foo.com,baz.com` | No       |

## Outbound Allow-list

For enterprise plan, you must allow several outbound connections for license checking. Please contact support@flowiseai.com for more information.
