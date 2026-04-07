
<html>
<head>
  <title>HCJ</title>

  <style>
    body {
      background: radial-gradient(circle, #000000, #020012);
      color: #00ffcc;
      font-family: monospace;
      text-align: center;
    }

    h2 {
      text-shadow: 0 0 15px #00ffcc;
    }

    button {
      padding: 10px 20px;
      margin: 10px;
      background: black;
      color: #00ffcc;
      border: 1px solid #00ffcc;
      box-shadow: 0 0 10px #00ffcc;
      cursor: pointer;
    }

    button:hover {
      background: #00ffcc;
      color: black;
      box-shadow: 0 0 20px #00ffcc;
    }

    input {
      margin: 10px;
      color: white;
    }

    canvas {
      border: 1px solid #00ffcc;
      box-shadow: 0 0 10px #00ffcc;
    }
  </style>
</head>

<body>

<h2>⚡ HCJ v3 File → Image Encoder ⚡</h2>

<input type="file" id="fileInput"><br>

<button onclick="encode()">🚀 Encode File</button><br>

<input type="file" id="decodeInput"><br>

<button onclick="decode()">🔓 Decode Image</button>

<br><br>
<canvas id="canvas"></canvas>

<script>

// FILE → IMAGE
function encode() {
  let file = document.getElementById("fileInput").files[0];
  if (!file) return alert("Select file!");

  let reader = new FileReader();

  reader.onload = function(e) {
    let bytes = new Uint8Array(e.target.result);

    let canvas = document.getElementById("canvas");
    let ctx = canvas.getContext("2d");

    let size = Math.ceil(Math.sqrt(bytes.length));
    canvas.width = size;
    canvas.height = size;

    let imgData = ctx.createImageData(size, size);
    let data = imgData.data;

    for (let i = 0; i < bytes.length; i++) {
      data[i * 4] = bytes[i];     // R
      data[i * 4 + 1] = 0;
      data[i * 4 + 2] = 0;
      data[i * 4 + 3] = 255;
    }

    ctx.putImageData(imgData, 0, 0);

    let link = document.createElement("a");
    link.download = "hcj_file.png";
    link.href = canvas.toDataURL();
    link.click();
  };

  reader.readAsArrayBuffer(file);
}


// IMAGE → FILE
function decode() {
  let file = document.getElementById("decodeInput").files[0];
  if (!file) return alert("Select image!");

  let img = new Image();
  let canvas = document.getElementById("canvas");
  let ctx = canvas.getContext("2d");

  img.onload = function() {
    canvas.width = img.width;
    canvas.height = img.height;

    ctx.drawImage(img, 0, 0);

    let data = ctx.getImageData(0, 0, canvas.width, canvas.height).data;

    let bytes = [];

    for (let i = 0; i < data.length; i += 4) {
      if (data[i] === 0) break;
      bytes.push(data[i]);
    }

    let blob = new Blob([new Uint8Array(bytes)]);
    let link = document.createElement("a");

    link.href = URL.createObjectURL(blob);
    link.download = "decoded_file";
    link.click();
  };

  img.src = URL.createObjectURL(file);
}

</script>

</body>
</html>
