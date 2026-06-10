<html>
<body style="margin:0; background:black;">

<video id="preview" autoplay playsinline style="width:100%; height:100%; object-fit:cover;"></video>

<button id="startRec" style="width:100%; height:60px; font-size:20px;">⚪︎</button>
<button id="stopRec" style="width:100%; height:60px; font-size:20px;">×</button>

<script>
let stream;
let recorder;
let chunks = [];

// カメラ起動（外カメラ）
async function startCamera() {
  stream = await navigator.mediaDevices.getUserMedia({
    video: { facingMode: "environment" },
    audio: true
  });
  document.getElementById("preview").srcObject = stream;
}

// 録画開始
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
    document.getElementById("startRec").style.display = "block";
  };
  document.getElementById("startRec").style.display = "none";
  recorder.start();
};

// 録画停止
document.getElementById("stopRec").onclick = () => recorder.stop();

startCamera();
</script>

</body>
</html>
