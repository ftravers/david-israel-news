# israel-news.jetsam.dev deployment

The static page is served from this repository by `israel-news-origin.service` on loopback port 18766. `israel-news-jetsam-dev.service` publishes that origin through a Cloudflare named tunnel.

Runtime-only tunnel configuration and credentials live under:

`~/.config/cloudflared-tunnels/israel-news-jetsam-dev/`

They must never be committed. Both services are installed as user units under `~/.config/systemd/user/` and enabled for the default user target.

The public Cloudflare lockdown rule explicitly allows `israel-news.jetsam.dev`; general scanner-path and scanner-agent blocks remain in force.

## Verification

```sh
systemctl --user show israel-news-origin.service -p ActiveState -p SubState -p NRestarts
systemctl --user show israel-news-jetsam-dev.service -p ActiveState -p SubState -p NRestarts
curl -fsS https://israel-news.jetsam.dev/ | grep -F 'David’s Israel News Guide'
```

## Rollback

The independent GitHub Pages copy remains available at:

https://ftravers.github.io/david-israel-news/

To take the jetsam.dev route offline, stop and disable `israel-news-jetsam-dev.service`, remove only the exact `israel-news.jetsam.dev` DNS route after confirming its tunnel target, then delete the named tunnel. Restore the Cloudflare lockdown expression by removing only the exact `israel-news.jetsam.dev` hostname exception.
