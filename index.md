# Home
## Why?
[Matrix](https://matrix.org) is a federated platform for exchanging JSON
messages in chat-like rooms. It is generally used for IM clients and for WebRTC
call setup, but it's powerful architecture has yet to become frequently used for
custom real-time applications.

There's one problem, though: It's quite cumbersome (and insecure) to sign in to
every app that you would like to use. For Matrix-based webapps to catch on,
there needs to be a universal way to authorize an app to have *limited* access
to an account.

OAuth could be used to do this, but OAuth would require that each application
run servers and it would require that the user grant 24/7 access to their
account, even when their computer isn't open. It would be best to have a way to
grant a *static* webapp limited access to a user's account. That's where mxapps
comes in.

## Architecture
`mxapps` is composed of three parts:
* [`mx-host-core-worker` (GitHub)](https://github.com/KB1RD/mx-host-core-worker/)
does all of the heavy-lifting and maintains application state. This is a single
instance that is shared across multiple tabs using a shared or service worker.
It is written in TypeScript and has its own tests. **Both mxapp instances and
third-party webapps connect to this worker.** The worker itself is fairly
modular and is composed of various services that can be requested with a
specific semver version by clients and mxapp tabs.
* [`mxapps` (GitHub)](https://github.com/KB1RD/mxapps/) is the UI layer. It
maintains exclusively the state necessary for keeping the channel open and
navigation state. It relies on a functioning `mx-host-core-worker` to provide
all data and does no communication on its own. When unable to start the worker,
it will error out. It is written in Vue.JS and JavaScript, though all of the
code drives the UI. It currently has no automated tests
* The user apps themselves. The defining characteristic of an app is its
manifest JSON file. This contains attributes like title, description, and
version. It also contains a list of permissions that the app would like granted
and a list of entry points, such as an entry point for when a file is opened
with the app. Communication is established through one of two methods: Opening
the app, which will load an `iframe`, which will `postMessage` a `MessagePort`
back to the app. Another method is to embed the app itself in an `iframe` and
`postMessage` directly. The former is preferred since this provides the proper
UX of being on another website. See the [bootstrapping](/bootstrap) page.

## Browser Support

This table needs a bit of work, but here it is anyway:

| Browser        | Support           | Min Version |
|----------------|-------------------|-------------|
| Firefox        | Full              | ?           |
| Chrom(e/ium)   | Full              | ?           |
| Edge *1        | Probably?         | N/A         |
| Safari Mac     | Future *2         | N/A         |
| Safari iOS     | Future *2         | N/A         |
| Opera          | Unknown?          | N/A         |
| Android        | Unknown?          | N/A         |
| Android Chrome | Unknown?          | N/A         |
| Android FF     | Unknown?          | N/A         |

1. While Edge is a Chromium variant, AFAIK there are several differences that
could be important. I need to do more research.
2. WebKit support for service/shared workers is really, really weird. They first
supported shared workers in versions 5-6
([caniuse.com](https://caniuse.com/#feat=sharedworkers), but *dropped* support.
Service workers are supported, but with a big fat asterisk:
  
    > To prevent user cross-site tracking, WebKit further partitions service workers
    and service worker clients by the top level document origin, the origin shown in
    the address bar. Frames that are same origin as the top level document will
    behave the same as in other browser engines. WebKit behaves differently for
    cross-origin frames. A service worker registered by an example.com iframe inside
    a webkit.org page will be able to communicate to all service workers and clients
    that share the same (webkit.org, example.com) partition, but not to any other
    example.com or webkit.org client. A network load made by a (webkit.org,
    example.com) service worker will use the same cookies as if the network load was
    made in an example.com frame embedded in a webkit.org page. Similarly, private
    browsing mode is enforced in a service worker by partitioning service workers
    according the browsing session.
  
    This means that the first method of [bootstrapping](/bootstrap) apps
    (embedding `mxapps` in a hidden `iframe` on the app's page to talk to the
    service worker) will not work. I think that the only way to solve this is to
    embed the app itself in an iframe, which I don't like because it confuses
    the user by circumventing the normal functionality of the URL bar and may
    confuse people with screen readers. This confusion creates a potential
    security risk, as detailed in
    [this](https://stackoverflow.com/a/9428051/7853604) post.

