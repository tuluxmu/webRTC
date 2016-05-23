#getUserMedia

```
var video = document.querySelector('video');
var constraints = window.constraints = {
  audio: false,
  video: true
};
navigator.mediaDevices.getUserMedia(constraints)
.then(function(stream) {
var videoTracks = stream.getVideoTracks();
window.stream = stream; // make variable available to browser console
video.srcObject = stream;
.catch(function(error) {
});
```

#getUserMedia ⇒ canvas

how to spanShot  ？？

```
var button = document.querySelector('button');
button.onclick = function() {
  canvas.width = video.videoWidth;
  canvas.height = video.videoHeight;
  canvas.getContext('2d').
    drawImage(video, 0, 0, canvas.width, canvas.height);
};
```

#getUserMedia + CSS filters

add ClassName

```
.none {
      -webkit-filter: none;
      filter: none;
    }
    .blur {
      -webkit-filter: blur(3px);
      filter: blur(3px);
    }
    .grayscale {
      -webkit-filter: grayscale(1);
      filter: grayscale(1);
    }
    .invert {
      -webkit-filter: invert(1);
      filter: invert(1);
    }
    .sepia {
      -webkit-filter: sepia(1);
      filter: sepia(1);
    }
    ```
#adapt the size of screen

```
var qvgaConstraints = {
  video: {width: {exact: 320}, height: {exact: 240}}
};
function getMedia(constraints) {
  if (stream) {
    stream.getTracks().forEach(function(track) {
      track.stop();
    });
  }
  setTimeout(function() {
    navigator.mediaDevices.getUserMedia(
      constraints
    ).then(
      successCallback,
      errorCallback
    );
  }, (stream ? 200 : 0));
}
```

#Select sources & outputs

Get available audio, video sources and audio output devices* from mediaDevices.enumerateDevices() then set the source for getUserMedia() using a deviceId constraint.

```
navigator.mediaDevices.enumerateDevices()
.then(gotDevices)
.catch(errorCallback);

```
#getUserMedia, audio only

#MediaRecorder
#peerconnection

```
function call() {
  callButton.disabled = true;
  hangupButton.disabled = false;
  trace('Starting call');
  startTime = window.performance.now();
  var videoTracks = localStream.getVideoTracks();
  var audioTracks = localStream.getAudioTracks();
  if (videoTracks.length > 0) {
    trace('Using video device: ' + videoTracks[0].label);
  }
  if (audioTracks.length > 0) {
    trace('Using audio device: ' + audioTracks[0].label);
  }
  var servers = null;
  pc1 = new RTCPeerConnection(servers);
  trace('Created local peer connection object pc1');
  pc1.onicecandidate = function(e) {
    onIceCandidate(pc1, e);
  };
  pc2 = new RTCPeerConnection(servers);
  trace('Created remote peer connection object pc2');
  pc2.onicecandidate = function(e) {
    onIceCandidate(pc2, e);
  };
  pc1.oniceconnectionstatechange = function(e) {
    onIceStateChange(pc1, e);
  };
  pc2.oniceconnectionstatechange = function(e) {
    onIceStateChange(pc2, e);
  };
  pc2.onaddstream = gotRemoteStream;

  pc1.addStream(localStream);
  trace('Added local stream to pc1');

  trace('pc1 createOffer start');
  pc1.createOffer(
    offerOptions
  ).then(
    onCreateOfferSuccess,
    onCreateSessionDescriptionError
  );
}
```

#Peer connection: audio only

```
function call() {
  callButton.disabled = true;
  codecSelector.disabled = true;
  trace('Starting call');
  var servers = null;
  var pcConstraints = {
    'optional': []
  };
  pc1 = new RTCPeerConnection(servers, pcConstraints);
  trace('Created local peer connection object pc1');
  pc1.onicecandidate = iceCallback1;
  pc2 = new RTCPeerConnection(servers, pcConstraints);
  trace('Created remote peer connection object pc2');
  pc2.onicecandidate = iceCallback2;
  pc2.onaddstream = gotRemoteStream;
  trace('Requesting local stream');
  navigator.mediaDevices.getUserMedia({
    audio: true,
    video: false
  })
  .then(gotStream)
  .catch(function(e) {
    alert('getUserMedia() error: ' + e.name);
  });
}
```

#Multiple peer connections

```
function call() {
  callButton.disabled = true;
  hangupButton.disabled = false;
  trace('Starting calls');
  
  var audioTracks = window.localstream.getAudioTracks();
  var videoTracks = window.localstream.getVideoTracks();
  if (audioTracks.length > 0) {
    trace('Using audio device: ' + audioTracks[0].label);
  }
  if (videoTracks.length > 0) {
    trace('Using video device: ' + videoTracks[0].label);
  }
  
  // Create an RTCPeerConnection via the polyfill.
  var servers = null;
  pc1Local = new RTCPeerConnection(servers);
  pc1Remote = new RTCPeerConnection(servers);
  pc1Remote.onaddstream = gotRemoteStream1;
  pc1Local.onicecandidate = iceCallback1Local;
  pc1Remote.onicecandidate = iceCallback1Remote;
  trace('pc1: created local and remote peer connection objects');

  pc2Local = new RTCPeerConnection(servers);
  pc2Remote = new RTCPeerConnection(servers);
  pc2Remote.onaddstream = gotRemoteStream2;
  pc2Local.onicecandidate = iceCallback2Local;
  pc2Remote.onicecandidate = iceCallback2Remote;
  trace('pc2: created local and remote peer connection objects');

  pc1Local.addStream(window.localstream);
  trace('Adding local stream to pc1Local');
  pc1Local.createOffer(
    offerOptions
  ).then(
    gotDescription1Local,
    onCreateSessionDescriptionError
  );

  pc2Local.addStream(window.localstream);
  trace('Adding local stream to pc2Local');
  pc2Local.createOffer(
    offerOptions
  ).then(
    gotDescription2Local,
    onCreateSessionDescriptionError
  );
}
```

#Peer connection relay

```
pipes.push(new VideoPipe(localStream, gotremoteStream));
```

# Munge SDP

```
get media
create peer connection
create offer
set offer
create answer
set answer
hang up
```

#Use pranswer when setting up a peer connection

# createOffer() output
#Send DTMF tones

# Peer connection: states

```
PC1 state:stable => have-local-offer => stable => closed
PC1 ICE state:new => checking => connected => completed => completed
PC2 state:stable => have-remote-offer => stable => closed
PC2 ICE state:new => checking => connected => connected
```

#Trickle ICE

