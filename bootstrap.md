# App Bootstrapping

This is the process by which an app obtains a communication channel with
`mx-host-core-worker` to access the API set. **This documentation is a very
rough draft and may not make much sense yet.**

## Methods

### Linkback Method

This is the most complex method, but provides the best UX. It goes as follows:

1. A tab with `mxapps` open obtains a token and provides a context object
2. A new tab is created with the URL provided in the manifest's `entry_points`
A `return_url` is substituted into the provided URL. The `return_url` a path
within the `mxapp` instance and contains the token. (See `return_url` below)
3. The app loads in a new tab and opens the `return_url` in a *hidden* iframe
4. The `mxapps` instance loaded in the iframe gets a MessagePort and the context
object from `mx-host-core-worker` and posts them to the iframe parent. The
message is posted as a simple array in this format:
    ```javascript
    ['ctx', { port, context }, error]
    ```
    ... where `'ctx'` is a **fixed** string, `port` is a MessagePort, `context`
    is the context object, and `error` is an error object. **The middle object
    may be undefined**
5. The app picks up this message and completes the setup. At this point, the app
should remove the iframe from the DOM

### Embed Method

**This method is not yet implemented anywhere**

1. A tab with `mxapps` open obtains a token and provides a context object
2. A new tab is created with the URL provided in the manifest's `entry_points`
A `return_url` of `_` is substituted into the URL instead of an actual URL
3. Upon loading, the app `postMessage`s `['ready']` to the parent
4. The parent sends back a message in the same format as above with port and
context information.

## `return_url`

Passing a `return_url` is the method by which an app knows which instance to
contact when instances may be self-hosted and the method by which a single-use
token is verified. The URL provided in the manifest's `entry_points` contains
the text `{{return}}` **exactly** wherever the `return_url` should go. The URL
will be, of course, encoded as a URI component using `encodeURIComponent`.

## Context

The context object is a bit of information passed to the permission system and
the app that provides all of the necessary information to access the right
endpoints. The minimum required context is as follows:
```typescript
{
  /**
   * Account ID to grant access to
   */
  account_id: string
  /**
   * Application being given access
   */
  app_url: string
}
```
When an app is used to open a room (currently, this is the only way to open an
app), the context is as follows:
```typescript
{
  /**
   * Account ID to grant access to
   */
  account_id: string
  /**
   * Application being given access
   */
  app_url: string
  /**
   * Room being given access to (if any)
   */
  room_id: string
}
```

