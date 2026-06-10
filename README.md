<!DOCTYPE html>
<html>
<body>
<video id="preview" autoplay playsinline style="width:100%;"></video>
<button id="startRec">録画開始</button>
<button id="stopRec">録画停止</button>

<script>
let stream;
let recorder;
let chunks = [];

async function startCamera() {
  stream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true });
  document.getElementById("preview").srcObject = stream;
}

document.getElementById("startRec").onclick = () => {
  chunks = [];
  recorder = new MediaRecorder(stream, { mimeType: "video/webm" });
  recorder.ondataavailable = e => chunks.push(e.data);
  recorder.onstop = () => {
    const blob = new Blob(chunks, { type: "video/webm" });
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = "recorded.webm";
    a.click();
  };
  recorder.start();
};

document.getElementById("stopRec").onclick = () => recorder.stop();

startCamera();
</script>
</body>
</html>
