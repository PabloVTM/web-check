<p align="center">
  <img width="72" src="./.github/web-check-logo.png" alt="Web Check logo" />
</p>

# Web Check (fork)

An all-in-one OSINT and security tool that reveals how any website works: IP and DNS, SSL chain, headers, cookies, DNSSEC, mail config, open ports, trackers, redirects, security policies, archives, performance and more.

This is a fork of [lissy93/web-check](https://github.com/lissy93/web-check) by [Alicia Sykes](https://aliciasykes.com), maintained at [PabloVTM/web-check](https://github.com/PabloVTM/web-check) with the PabloVTM terminal theme shared with the other PabloVTM tools.

![Screenshot](./.github/screenshots/web-check-screenshot10.png)

## Image

Images are published to GitHub Container Registry for `linux/amd64` and `linux/arm64`:

| Tag | Content |
| --- | --- |
| `ghcr.io/pablovtm/web-check:latest` | Latest build of `master`. |
| `ghcr.io/pablovtm/web-check:YYYY-MM-DD` | Dated version, e.g. `2026-11-08`. A new one is built every Monday with the latest Debian and Chromium security updates. |

## Quick start

```yaml
services:
  web-check:
    container_name: web-check
    image: ghcr.io/pablovtm/web-check:latest
    ports:
      - 3000:3000
    restart: unless-stopped
```

or

```bash
docker run -d --name web-check -p 3000:3000 ghcr.io/pablovtm/web-check:latest
```

Then open http://localhost:3000. The container reports its health on `/healthz`.

Web Check makes outbound requests to the sites you scan and to the public APIs some checks use. If you expose it, put it behind authentication and consider `API_ENABLE_RATE_LIMIT` and `API_BLOCKED_HOSTS` (to stop scans of your internal network).

## Configuration

Everything is optional. Pass variables with `-e` / `environment:`, or put them in a `.env` file when running from source.

**API keys** (enable or improve some checks):

| Variable | Purpose |
| --- | --- |
| `GOOGLE_CLOUD_API_KEY` | PageSpeed Insights and Safe Browsing checks ([get one](https://developers.google.com/speed/docs/insights/v5/get-started)). |
| `SHODAN_API_KEY` | Host names, server info and vulnerabilities checks ([get one](https://account.shodan.io/)). |
| `CLOUDMERSIVE_API_KEY` | Adds the Cloudmersive scan to the threats check ([get one](https://account.cloudmersive.com/)). |
| `TRANCO_API_KEY` / `TRANCO_USERNAME` | Higher Tranco rate limit for the rank check ([get one](https://tranco-list.eu/)). |
| `GITHUB_TOKEN` | Higher GitHub rate limit for the social presence check. |
| `CERTSPOTTER_TOKEN` | Higher CertSpotter rate limit for the subdomains check ([get one](https://sslmate.com/certspotter/api/)). |

**Settings**:

| Variable | Purpose |
| --- | --- |
| `PORT` | Port to listen on (default `3000`). |
| `API_ENABLE_RATE_LIMIT` | Rate-limit the `/api` endpoints (`true`). |
| `PUBLIC_API_TIMEOUT_LIMIT` | API request timeout in milliseconds, e.g. `25000`. |
| `API_CORS_ORIGIN` | Allowed CORS origin(s), e.g. `example.com`. |
| `API_DISABLED_CHECKS` | Checks to disable, comma separated, e.g. `trace-route,ports`. |
| `API_ENABLED_CHECKS` | If set, only these checks run, e.g. `get-ip,ssl,dns,headers`. |
| `API_BLOCKED_HOSTS` | Hosts that must never be scanned, e.g. `lan.example.com,192.168.0.0/16`. |
| `CHROME_PATH` | Chromium executable (set in the image to `/usr/bin/chromium`). |
| `DISABLE_GUI` | Serve only the API (`true`). |
| `PUBLIC_API_ENDPOINT` | API endpoint used by the frontend, e.g. `/api`. |

Variables prefixed with `PUBLIC_` are read by the frontend at build time: rebuild the image after changing them.

## Building

```bash
docker build -t web-check:local .
```

## Development

Requires Node.js 22 or 24, Yarn and Git. Some checks also need `chromium`, `traceroute` and `dig`; they are skipped when missing.

```bash
yarn install     # install dependencies
yarn dev         # API on :3001 and frontend with hot reload
yarn build       # production build in dist/
yarn start       # run the built app (API and GUI)
yarn lint        # ESLint
yarn typecheck   # astro check
yarn format:check
```

## Credits

Web Check is created and maintained by [Alicia Sykes](https://aliciasykes.com) and its [contributors](https://github.com/lissy93/web-check/graphs/contributors). If you find it useful, consider [sponsoring Alicia](https://github.com/sponsors/Lissy93). This fork is maintained by [PabloVTM](https://github.com/PabloVTM).

## License

[MIT](LICENSE) © Alicia Sykes.
