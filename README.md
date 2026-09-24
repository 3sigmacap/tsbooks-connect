# tsbooks-connect

The https return address for connecting QuickBooks Online to
[TSBooks](https://github.com/3sigmacap/TSBooks) running on a home network.

Intuit only sends people back to `https://` addresses after they approve a
connection, never to a home-network address like `http://192.168.1.20:3000`.
This one static page, served by GitHub Pages at

    https://3sigmacap.github.io/tsbooks-connect/

is registered with Intuit as the app's production redirect URI. It forwards
Intuit's answer to the TSBooks server that started the connection:

1. TSBooks sends you to Intuit with `state = <nonce>.<base64url(TSBooks origin)>`.
2. Intuit sends you here with `?code=…&state=…&realmId=…`.
3. This page forwards those parameters to `<TSBooks origin>/api/qbo/callback`.

**Why this is safe to host publicly**

- It holds no keys and stores nothing.
- The code on its own is useless: exchanging it needs the app's client secret and
  a one-time PKCE verifier, and both stay on the TSBooks server. TSBooks also
  checks `state` against a cookie in your browser.
- It only forwards to private-network addresses (localhost, 10.x, 172.16–31.x,
  192.168.x, 100.64/10, `.local`, `.lan`, `.home.arpa`, `.ts.net` and
  single-word host names), so it can't be used to redirect anyone to an
  outside site.
