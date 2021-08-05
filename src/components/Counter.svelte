<style>
video {
  display: none;
  width: calc(100% - 2em);
}
canvas {
  margin: 1em;
  width: calc(100% - 2em);
}
</style>

<script>
// width of square for video
const DIMENSION = 217;

import { onMount } from "svelte";
import { onDestroy } from "svelte";

// props for current exercise
export let image;
export let title = "Exercise";
export let description;
export let model;
export let id;
//interval object used for keeping repeating predictions every few miliseconds
let timer;

let status = "up";

let canvas;
let context;
let video;
let videoScale;
let xOffset;
let yOffset;
let stream = null;

let isSetup = false;
let started = false;

let count = 0;
//helper functions
const likeliest = (predictions) =>
  predictions.reduce((a, b) => (a.probability > b.probability ? a : b));
// needed when depugging locally with front end and back end on different ports
// [protocol]//[hostname][:port]/predict
// [http:]//[example.com]/predict
const apiEndpoint = `${window.location.protocol}//${window.location.hostname}${
  Boolean(window.location.port) ? ":3000" : ""
}/predict`;
// stop timer and video when nolonger needed
const cleanUp = () => {
  started = false;
  isSetup = false;
  // timer && clearInterval(timer);
  timer = false;
  stream && stream.getVideoTracks()[0].stop();
};

function interpret(predictions) {
  const { probability, className } = likeliest(predictions);
  if (probability > 0.7) {
    switch (className) {
      case "up":
      case "down":
        if (className !== status) {
          count += 0.5;
          status = className;
        }
        break;
      default:
        break;
    }
  }
}

async function getPrediction() {
  videoScale = Math.min(video.videoHeight, video.videoWidth);
  xOffset = (((videoScale - video.videoWidth) / 2) * DIMENSION) / videoScale;
  yOffset = (((videoScale - video.videoHeight) / 2) * DIMENSION) / videoScale;

  context.drawImage(
    video,
    xOffset,
    yOffset,
    DIMENSION - 2 * xOffset,
    DIMENSION - 2 * yOffset
  );

  return await fetch(apiEndpoint, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      title,
      id,
      model,
      image: canvas.toDataURL("image/jpeg"),
    }),
  })
    .then((response) => response.json())
    .then(interpret);
}
const sequentialTrack = () =>
  //use setTimeout sequentialTrack is added as a task rather than microtask
  // this ensures starvation does not occur
  getPrediction().then(() => (timer ? setTimeout(sequentialTrack, 0) : null));
async function track() {
  timer = true;
  // getPrediction();
  // let t = performance.now();
  sequentialTrack();

  //.then(() => (timer ? setInterval(getPrediction, 100) : null));
  // timer = setInterval(getPrediction, 100);
}

onMount(() => {
  context = canvas.getContext("2d");
});

onDestroy(cleanUp);

//sets up video stream
async function setup() {
  try {
    stream = await navigator.mediaDevices.getUserMedia({
      audio: false,
      video: true,
    });
    video.srcObject = stream;
  } catch (error) {
    console.log("error setting up video", error);
  }
}
async function toggleRecording(event) {
  if (started) {
    cleanUp();
  } else {
    started = !started;
    await setup();
    track();
  }
  event.preventDefault();
}
</script>

<div class="card mb-3">
  <div class="row g-0">
    <div class="col-md-4 col-sm-6">
      {#if !started}
        <img
          src="{'/assets/' + image}"
          class="img-fluid rounded-start"
          alt="..." />
      {/if}
      <!-- svelte-ignore a11y-media-has-caption -->

      <video class="m-3" bind:this="{video}" playsinline autoplay></video>

      <canvas
        bind:this="{canvas}"
        width="{217}"
        height="{217}"
        style="display:{started ? 'block' : 'none'}"></canvas>
    </div>
    <div class="col-md-8 col-sm-6">
      <div class="card-body">
        <h1 class="display-5 fw-bold">{title}</h1>
        <code class="display-5">{count} - {status}</code>
        <p class="col-md-8 fs-4">{description}</p>

        <button
          type="button"
          class="btn btn-primary btn-lg"
          on:click="{toggleRecording}"
          >{started ? `Stop` : `Start Tracking`}</button>
      </div>
    </div>
  </div>
</div>
