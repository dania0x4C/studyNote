```
const socket = io();

  

const myFace = document.getElementById("myFace");

const muteBtn = document.getElementById("mute");

const cameraBtn = document.getElementById("camera");

const camerasSelect = document.getElementById("cameras");

  

let myStream;

let muted = false;

let cameraOff = false;

  

async function getCameras() {

  try {

    const devices = await navigator.mediaDevices.enumerateDevices();

    const cameras = devices.filter(device => device.kind === "videoinput");

    const currentCamera = myStream.getVideoTracks()[0];

    cameras.forEach(camera => {

      const option = document.getElementById("option");

      option.value = camera.deviceId;

      option.innerText = camera.label;

      if(currentCamera.label === camera.label){

        option.selected = true;

      }

      camerasSelect.appendChild(option);

    });

  } catch (e) {

    console.log(e);

  }

}

  

async function getMedia(deviceId) {

  const initialConstrains = {

    audio: true,

    video: { facingMode: "user" },

  };

  const cameraConstraints = {

    audio: true,

    video: { deviceId: { exact: myExactCameraOrBustDeviceId } },

  };

  try {

    myStream = await navigator.mediaDevices.getUserMedia(

        deviceId ? cameraConstraints : initialConstrains

    );

    myFace.srcObject = myStream;

    if(! deviceId){

        await getCameras();

    }

  } catch (e) {

    console.log(e);

  }

}

  

getMedia();

  

function handleMuteClick() {

  myStream.getAudioTracks().foreach(track => (track.enabled = !track.enabled));

  if (!muted) {

    muteBtn.innerText = "Unmute";

    muted = true;

  } else {

    muteBtn.innerText = "mute";

    muted = false;

  }

}

function handleCameraClick() {

  myStream.getVideoTracks().foreach(track => (track.enabled = !track.enabled));

  if (!cameraOff) {

    cameraOff.innerText = "Turn Camera Off";

    cameraOff = true;

  } else {

    cameraOff.innerText = "Turn Camera On";

    cameraOff = false;

  }

}

  

async function handleCameraChange() {

  getMedia(camerasSelect.value);

}

  

muteBtn.addEventListener("click", handleMuteClick);

cameraBtn.addEventListener("click", handleCameraClick);

camerasSelect.addEventListener("input", handleCameraChange);
```

카메라 on, off 기능
카메라 변경 기능
sound on, off

구현

