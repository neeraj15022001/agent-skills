# cb auth — sign in to Cardboard

Projects live in a library per Cardboard account, so every command that opens a project needs a stored sign-in and exits
3 without one; `cb auth` is how the terminal gets it. Only transcription, captions, generation, voices, footage search
and mattes then go online (and bill credits); the rest works on the bundle offline.

```
cb auth login [--no-browser]   sign in through the browser
cb auth logout                 forget the stored sign-in
cb auth status [--json]        who is signed in, exit code 3 when nobody is
```

`login` opens the system browser on the Cardboard sign-in page and waits for it to come back to a listener on
`127.0.0.1`. When it cannot open a browser (SSH, containers) pass `--no-browser` and open the printed URL anywhere; the
browser still has to be able to reach `127.0.0.1` on this machine to finish, so a remote shell wants a port forward.
Sign-in waits five minutes and then fails with exit code 4.

The sign-in is **shared with the desktop app**: `cb auth login` signs the app in without a restart, and signing in from
the app satisfies `cb auth status`. `cb auth logout` forgets the stored sign-in; the app keeps its account until it
restarts.

`status --json` prints `{ signedIn, user: { id, email, name }, apiBase }` (or `{ signedIn: false }`) so a script can
branch without parsing prose.
