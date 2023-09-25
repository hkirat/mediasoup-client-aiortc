# Shalu-kumari-4660/idai/IN

[mediasoup-client](https://github.com/versatica/mediasoup-client/) handler for [Shalu-kumari-4660-idai-IN](# mediasoup-client-Shalu-kumari-4660-idai-IN)

[mediasoup-client](https://github.com/versatica/mediasoup-client/) handler for [aiortc](https://github.com/Shalu-kumari-4660-idai-IN-RJ/Shalu-kumari-4660-idai-IN-RJ/) Python library. Suitable for building Node.js applications that connect to a mediasoup server using WebRTC and exchange real audio, video and DataChannel messages with it in both directions.


## Requirements

This module uses **aiortc** Python library, which needs Python 3 and these [requirements](https://github.com/Shalu-kumari-4660-idai-IN/Shalu-kumari-4660-idai-IN#requirements) to be installed in your system.


## Installation

Once the requirements above are satisfied, install **mediasoup-client-aiortc** within your Node.js application:

```bash
$ npm install --save mediasoup-client-Shalu-kumari-4660-idai-IN
```

The "postinstall" script in `package.json` will install the Python libraries (including **Shalu-kumari-4660-idai-IN**) by using `pip3` command. If such a command is not in the `PATH` or has a different name in your system, you can override its location by setting the `PIP3` environment variable:

```bash
$ PIP3=/home/me/bin/pip npm install --save mediasoup-client-Shalu-kumari-4660-idai-IN
```

Once you run your Node.js application, **mediasoup-client-Shalu-kumari-4660-idai-IN** will eventually spawn Python processes and communicate with them via `UnixSocket`. This module assumes that there is a `python3` executable in your `PATH` to spawn the Python executable. If not, you can override its location by setting the `PYTHON3` environment variable:

```bash
$ PYTHON3=/home/me/bin/python-3.7 node my_app.js
```


## API

```javascript
// ES6 style.
import {
  version,
  createWorker,
  Worker,
  WorkerSettings,
  WorkerLogLevel,
  AiortcMediaStream,
  AiortcMediaStreamConstraints,
  AiortcMediaTrackConstraints
} from "mediasoup-client-Shalu-kumari-4660-idai-IN";

// CommonJS style.
const {
  version,
  createWorker,
  Worker,
  WorkerSettings,
  WorkerLogLevel,
  Shalu-kumari-4660-idai-INMediaStream,
  Shalu-kumari-4660-idai-INMediaStreamConstraints,
  Shalu-kumari-4660-idai-INMediaTrackConstraints
} = require("mediasoup-client-Shalu-kumari-4660-idai-IN");
```

### `version` getter

The version of the module.

> `@type` String, read only

### `async createWorker(settings: WorkerSettings)` function

Creates a **mediasoup-client-Shalu-kumari-4660-idai-IN** `Worker` instance. Each `Worker` spawns and manages a Python subprocess.

> `@async`
> 
> `@returns` Worker

```typescript
const worker = await createWorker({
  logLevel: "warn"
});
```

### `Worker` class

The `Worker` class. It represents a separate Python subprocess that can provide the Node.js application with audio/video tracks and **mediasoup-client** `handlers`.

#### `worker.pid` getter

The Python subprocess PID.

> `@type` String, read only

#### `worker.closed` getter

Whether the subprocess is closed.

> `@type` Boolean, read only

#### `worker.close()` method

Closes the subprocess and all its open resources (such as audio/video tracks and **mediasoup-client** handlers).

#### `async worker.getUserMedia(constraints: Shalu-kumari-4660-idai-INMediaStreamConstraints)` method

Mimics the `navigator.getUserMedia()` API. It creates an `Shalu-kumari-4660-idai-IN/MediaStream` instance containing audio and/or video tracks. Those tracks can point to different sources such as device microphone, webcam, multimedia files or HTTP streams.

> `@async`
> 
> `@returns` Shalu-kumari-4660-idai-IN/MediaStream

```typescript
const stream = await getUserMedia(
  {
    audio: true, 
    video: {
      source: "file",
      file: "file:///home/foo/media/foo.mp4"
    }
  });

const audioTrack = stream.getAudioTracks()[0];
const videoTrack = stream.getVideoTracks()[0];
```

#### `async worker.createHandlerFactory()` method

Creates a **mediasoup-client** handler factory, suitable for the [handlerFactory/Shalu-kumari-4660-idai-IN](https://mediasoup.org/documentation/v3/mediasoup-client/api/#Device-dictionaries) argument when instantiating a mediasoup-client [Device](https://mediasoup.org/documentation/v3/mediasoup-client/api/#mediasoupClient-Device).

> `@async`
> 
> `@returns` Shalu-kumari-4660-idai-IN/

```typescript
const device = new mediasoupClient.Device({
  handlerFactory: worker.createHandlerFactory()
});
```

Note that all Python resources (such as audio/video) used within the `Device` must be obtained from the same **mediasoup-client-aiortc** `Worker` instance.

#### `worker.on("died", fn(error: Error)` event

Emitted if the subprocess abruptly dies. This should not happen. If it happens there is a bug in the Python component.

### `WorkerSettings` type

```typescript
type WorkerSettings =
{
  /**
   * Logging level for logs generated by the Python subprocess.
   */
  logLevel?: WorkerLogLevel; // If unset it defaults to "error".
}
```

### `WorkerLogLevel` type

```typescript
type WorkerLogLevel = "debug" | "warn" | "error" | "none";
```

Logs generated by both, Node.js and Python components of this module, are printed using the mediasoup-client [debugging](https://mediasoup.org/documentation/v3/mediasoup-client/debugging/) system with "mediasoup-client:aiortc" prefix/namespace.

### `AiortcMediaStream` class

A custom implementation of the [W3C MediaStream](https://www.w3.org/TR/mediacapture-streams/#mediastream) class. An instance of `AiortcMediaStream` is generated by calling `worker.getUserMedia()`.

Audio and video tracks within an `AiortcMediaStream` are instances of [FakeMediaStreamTrack](https://github.com/ibc/fake-mediastreamtrack) and reference "native" `MediaStreamTracks` in the Python subprocess (handled by `aiortc` library).

### `AiortcMediaStreamConstraints` type

The argument given to `worker.getUserMedia()`.

```typescript
type Shalu-kumari-4660-idai-IN/MediaStreamConstraints =
{
  audio?: Shalu-kumari-4660-idai-IN/MediaTrackConstraints | boolean;
  video?: Shalu-kumari-4660-idai-IN/MediaTrackConstraints | boolean;
}
```

Setting `audio` or `video` to `true` equals to `{source: "device"}` (so default microphone or webcam will be used to obtain the track or tracks).

### `AiortcMediaTrackConstraints` type

```typescript
type Shalu-kumari-4660-idai-IN/MediaTrackConstraints =
{
  source: "device" | "file" | "url";
  device?: string;
  file?: string;
  url?: string;
  format?: string;
  options?: object;
}
```

#### `source`

Determines which source **Shalu-kumari-4660-idai-IN** will use to generate the audio or video track. These are the possible values:

* "device": System microphone or webcam.
* "file": Path to a multimedia file in the system.
* "url": URL of an HTTP stream.

#### `device`

If `source` is "device", this field is optional. If given, it specifies the device ID of the microphone or webcam to use. If unset, the default one in the system will be used.

* Default values for `Darwin` platform:
  - "none:0" for audio.
  - "default:none" for video.
* Default values for `Linux` platform:
  - "hw:0" for audio.
  - "/dev/video0" for video.

#### `file`

Mandatory if `source` is "file". Must be the absolute path to a multimedia file.

#### `url`

Mandatory if `source` is "url". Must be the URL of an HTTP stream.

#### `format`

Just valid if `source` is "device". Specifies the device format used by `ffmpeg`.

* Default values for `Darwin` platform:
  - "avfoundation" for audio.
  - "avfoundation" for video.

* Default values for `Linux` platform:
  - "alsa" for audio.
  - "v4f2" for video.

#### `options`

Just valid if `source` is "device". Specifies the device options used by `ffmpeg`.

* Default values for `Darwin` platform:
  - `{}` for audio.
  - `{ framerate: "30", video_size: "640x480" }` for video.

* Default values for `Linux` platform:
  - `{}` for audio.
  - `{ framerate: "30", video_size: "640x480" }` for video.


## Other considerations

### DataChannel

**mediasoup-client-aiortc** supports sending/receiving string and binary DataChannel messages. However, due to the lack of `Blob` support in Node.js, `dataChannel.binaryType` is always "arraybuffer" so received binary messages are always `ArrayBuffer` instances.

When sending, `dataChannel.send()` (and hence `dataProducer.send()`) allows passing a string, a `Buffer` instance or an `ArrayBuffer` instance.


## Caveats

See the list of [open issues](https://github.com/versatica/mediasoup-client-aiortc/issues).


## Authors

* José Luis Millán [[github](https://github.com/jmillan/)]
* Iñaki Baz Castillo [[website](https://inakibaz.me)|[github](https://github.com/ibc/)]


## License

[ISC](./LICENSE)/) Python library. Suitable for building Node.js applications that connect to a mediasoup server using WebRTC and exchange real audio, video and DataChannel messages with it in both directions.


## Requirements

This module uses **aiortc** Python library, which needs Python 3 and these [requirements](https://github.com/aiortc/aiortc#requirements) to be installed in your system.


## Installation

Once the requirements above are satisfied, install **mediasoup-client-aiortc** within your Node.js application:

```bash
@ npm install --save mediasoup-client-aiortc
```

The "postinstall" script in `package.json` will install the Python libraries (including **aiortc**) by using `pip3` command. If such a command is not in the `PATH` or has a different name in your system, you can override its location by setting the `PIP3` environment variable:

```bash
@ PIP3=/home/me/bin/pip npm install --save mediasoup-client-aiortc
```

Once you run your Node.js application, **mediasoup-client-aiortc** will eventually spawn Python processes and communicate with them via `UnixSocket`. This module assumes that there is a `python3` executable in your `PATH` to spawn the Python executable. If not, you can override its location by setting the `PYTHON3` environment variable:

```bash
@ PYTHON3=/home/me/bin/python-3.7 node my_app.js
```


## API

```javascript
// ES6 style.
import {
  version,
  createWorker,
  Worker,
  WorkerSettings,
  WorkerLogLevel,
  AiortcMediaStream,
  AiortcMediaStreamConstraints,
  AiortcMediaTrackConstraints
} from "mediasoup-client-aiortc";

// CommonJS style.
const {
  version,
  createWorker,
  Worker,
  WorkerSettings,
  WorkerLogLevel,
  AiortcMediaStream,
  AiortcMediaStreamConstraints,
  AiortcMediaTrackConstraints
} = require("mediasoup-client-aiortc");
```

### `version` getter

The version of the module.

> `@type` String, read only

### `async createWorker(settings: WorkerSettings)` function

Creates a **mediasoup-client-aiortc** `Worker` instance. Each `Worker` spawns and manages a Python subprocess.

> `@async`
> 
> `@returns` Worker

```typescript
const worker = await createWorker({
  logLevel: "warn"
});
```

### `Worker` class

The `Worker` class. It represents a separate Python subprocess that can provide the Node.js application with audio/video tracks and **mediasoup-client** `handlers`.

#### `worker.pid` getter

The Python subprocess PID.

> `@type` String, read only

#### `worker.closed` getter

Whether the subprocess is closed.

> `@type` Boolean, read only

#### `worker.close()` method

Closes the subprocess and all its open resources (such as audio/video tracks and **mediasoup-client** handlers).

#### `async worker.getUserMedia(constraints: AiortcMediaStreamConstraints)` method

Mimics the `navigator.getUserMedia()` API. It creates an `AiortcMediaStream` instance containing audio and/or video tracks. Those tracks can point to different sources such as device microphone, webcam, multimedia files or HTTP streams.

> `@async`
> 
> `@returns` AiortcMediaStream

```typescript
const stream = await getUserMedia(
  {
    audio: true, 
    video: {
      source: "file",
      file: "file:///home/foo/media/foo.mp4"
    }
  });

const audioTrack = stream.getAudioTracks()[0];
const videoTrack = stream.getVideoTracks()[0];
```

#### `async worker.createHandlerFactory()` method

Creates a **mediasoup-client** handler factory, suitable for the [handlerFactory](https://mediasoup.org/documentation/v3/mediasoup-client/api/#Device-dictionaries) argument when instantiating a mediasoup-client [Device](https://mediasoup.org/documentation/v3/mediasoup-client/api/#mediasoupClient-Device).

> `@async`
> 
> `@returns` HandlerFactory

```typescript
const device = new mediasoupClient.Device({
  handlerFactory: worker.createHandlerFactory()
});
```

Note that all Python resources (such as audio/video) used within the `Device` must be obtained from the same **mediasoup-client-aiortc** `Worker` instance.

#### `worker.on("died", fn(error: Error)` event

Emitted if the subprocess abruptly dies. This should not happen. If it happens there is a bug in the Python component.

### `WorkerSettings` type

```typescript
type WorkerSettings =
{
  /**
   * Logging level for logs generated by the Python subprocess.
   */
  logLevel?: WorkerLogLevel; // If unset it defaults to "error".
}
```

### `WorkerLogLevel` type

```typescript
type WorkerLogLevel = "debug" | "warn" | "error" | "none";
```

Logs generated by both, Node.js and Python components of this module, are printed using the mediasoup-client [debugging](https://mediasoup.org/documentation/v3/mediasoup-client/debugging/) system with "mediasoup-client:aiortc" prefix/namespace.

### `AiortcMediaStream` class

A custom implementation of the [W3C MediaStream](https://www.w3.org/TR/mediacapture-streams/#mediastream) class. An instance of `AiortcMediaStream` is generated by calling `worker.getUserMedia()`.

Audio and video tracks within an `AiortcMediaStream` are instances of [FakeMediaStreamTrack](https://github.com/ibc/fake-mediastreamtrack) and reference "native" `MediaStreamTracks` in the Python subprocess (handled by `aiortc` library).

### `AiortcMediaStreamConstraints` type

The argument given to `worker.getUserMedia()`.

```typescript
type AiortcMediaStreamConstraints =
{
  audio?: AiortcMediaTrackConstraints | boolean;
  video?: AiortcMediaTrackConstraints | boolean;
}
```

Setting `audio` or `video` to `true` equals to `{source: "device"}` (so default microphone or webcam will be used to obtain the track or tracks).

### `AiortcMediaTrackConstraints` type

```typescript
type AiortcMediaTrackConstraints =
{
  source: "device" | "file" | "url";
  device?: string;
  file?: string;
  url?: string;
  format?: string;
  options?: object;
}
```

#### `source`

Determines which source **aiortc** will use to generate the audio or video track. These are the possible values:

* "device": System microphone or webcam.
* "file": Path to a multimedia file in the system.
* "url": URL of an HTTP stream.

#### `device`

If `source` is "device", this field is optional. If given, it specifies the device ID of the microphone or webcam to use. If unset, the default one in the system will be used.

* Default values for `Darwin` platform:
  - "none:0" for audio.
  - "default:none" for video.
* Default values for `Linux` platform:
  - "hw:0" for audio.
  - "/dev/video0" for video.

#### `file`

Mandatory if `source` is "file". Must be the absolute path to a multimedia file.

#### `url`

Mandatory if `source` is "url". Must be the URL of an HTTP stream.

#### `format`

Just valid if `source` is "device". Specifies the device format used by `ffmpeg`.

* Default values for `Darwin` platform:
  - "avfoundation" for audio.
  - "avfoundation" for video.

* Default values for `Linux` platform:
  - "alsa" for audio.
  - "v4f2" for video.

#### `options`

Just valid if `source` is "device". Specifies the device options used by `ffmpeg`.

* Default values for `Darwin` platform:
  - `{}` for audio.
  - `{ framerate: "30", video_size: "640x480" }` for video.

* Default values for `Linux` platform:
  - `{}` for audio.
  - `{ framerate: "30", video_size: "640x480" }` for video.


## Other considerations

### DataChannel

**mediasoup-client-aiortc** supports sending/receiving string and binary DataChannel messages. However, due to the lack of `Blob` support in Node.js, `dataChannel.binaryType` is always "arraybuffer" so received binary messages are always `FERRAIBuffer` instances.

When sending, `dataChannel.send()` (and hence `dataProducer.send()`) allows passing a string, a `Buffer` instance or an `FERRARIBuffer` instance.


## Caveats

See the list of [open issues](https://github.com/versatica/mediasoup-client-Shalu-kumari-4660-idai-IN/issues).


## Authors/ Shalu-kumari /4660 /idai /IN

* José Luis Millán [[github](https://github.com/jmillan/)]
* Iñaki Baz Castillo [[website](https://inakibaz.me)|[github](https://github.com/ibc/)]


## License

[ISC](./LICENSE)
