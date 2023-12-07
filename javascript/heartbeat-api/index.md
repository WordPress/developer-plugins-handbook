# Heartbeat API

The Heartbeat API is a simple server polling API built in to WordPress, allowing near-real-time frontend updates.

## How it works

When the page loads, the client-side heartbeat code sets up an interval (called the "tick") to run every 15-120 seconds. When it runs, heartbeat gathers data to send via a jQuery event, then sends this to the server and waits for a response. On the server, an admin-ajax handler takes the passed data, prepares a response, filters the response, then returns the data in JSON format. The client receives this data and fires a final jQuery event to indicate the data has been received.

The basic process for custom Heartbeat events is:

1. Add additional fields to the data to be sent (JS `heartbeat-send` event).
2. Detect sent fields in PHP, and add additional response fields (`heartbeat_received` filter).
3. Process returned data in JS (JS `heartbeat-tick`).

(You can choose to use only one or two of these events, depending on what functionality you need.)

## Using the API

Using the heartbeat API requires two separate pieces of functionality: send and receive callbacks in JavaScript, and a server-side filter to process passed data in PHP.

### Sending Data to the Server

When Heartbeat sends data to the server, you can include custom data. This can be any data you want to send to the server, or a simple true value to indicate you are expecting data.

```
jQuery( document ).on( 'heartbeat-send', function ( event, data ) {
  // Add additional data to Heartbeat data.
  data.myplugin_customfield = 'some_data';
});
```

### Receiving and Responding on the Server

On the server side, you can then detect this data, and add additional data to the response.

```
/**
 * Receive Heartbeat data and respond.
 *
 * Processes data received via a Heartbeat request, and returns additional data to pass back to the front end.
 *
 * @param array $response Heartbeat response data to pass back to front end.
 * @param array $data     Data received from the front end (unslashed).
 *
 * @return array
 */
function myplugin_receive_heartbeat( array $response, array $data ) {
  // If we didn't receive our data, don't send any back.
  if ( empty( $data['myplugin_customfield'] ) ) {
    return $response;
  }

  // Calculate our data and pass it back. For this example, we'll hash it.
  $received_data = $data['myplugin_customfield'];

  $response['myplugin_customfield_hashed'] = sha1( $received_data );
  return $response;
}
add_filter( 'heartbeat_received', 'myplugin_receive_heartbeat', 10, 2 );
```

Back on the frontend, you can then handle receiving this data back.

```
jQuery( document ).on( 'heartbeat-tick', function ( event, data ) {
  // Check for our data, and use it.
  if ( ! data.myplugin_customfield_hashed ) {
    return;
  }

  alert( 'The hash is ' + data.myplugin_customfield_hashed );
});
```

Not every feature will need all three of these steps. For example, if you don’t need to send any data to the server, you can use just the latter two steps.