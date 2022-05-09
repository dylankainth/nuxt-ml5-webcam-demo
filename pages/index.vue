<template>
  <div class="container py-4">
    <header class="pb-3 mb-4 border-bottom">
      <a
        href="/"
        class="d-flex align-items-center text-dark text-decoration-none"
      >
        <span class="fs-4">ML5js Image Analysis Demo <span class="badge bg-primary">New</span> </span>
      </a>
    </header>

    <div class="p-5 my-4 bg-light rounded-3 shadow fadeintoview moverbox">
      <div class="container-fluid py-5">
        <h1 class="display-5 fw-bold title">Image Analysis</h1>
        <p class="col-md-8 fs-4 subtitle">
          See your webcam below and ML5.js will be used in your browser to guess
          the image
        </p>
      </div>
    </div>

    <div class="row align-items-md-stretch">
      <div class="col-md-6 layouthorizontal">
        <div
          class="video-wrapper h-100 p-5 bg-light rounded-3 shadow fadeintoview moverbox"
        >
          <video id="video" autoplay></video>
        </div>
      </div>
      <div class="col-md-6 layouthorizontal">
        <div class="h-100 p-5 bg-light rounded-3 shadow fadeintoview moverbox">
          <h2>Prediction</h2>
          <div v-if="!prediction && !error">
            <div class="spinner-border" role="status">
              <span class="visually-hidden">Loading...</span>
            </div>
          </div>
          <div v-if="error">An Error Occured: {{ error }}</div>
          <div v-if="prediction" class="fadeintoview">
            <h1 style="text-transform: capitalize">{{ prediction.label.slice(0, 20) + '...'}}</h1>
            <h1>{{ Math.round(prediction.confidence * 100) }}% confident</h1>

            <div class="progress">
              <div
                class="progress-bar"
                role="progressbar"
                :style="
                  'width:' +
                  Math.round(prediction.confidence * 100).toString() +
                  '%'
                "
                aria-valuemin="0"
                :aria-valuenow="Math.round(prediction.confidence * 100)"
                aria-valuemax="100"
              ></div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="p-5 my-4 bg-light rounded-3 shadow fadeintoview moverbox">
      <div class="container-fluid py-5">
        <h1 class="display-6 fw-bold">Details</h1>
        <span class="col-md-8 fs-5">
          <ul style="list-style: none">
            <li>
              <img
                src="https://ml5js.org/static/ml5_logo_purple-88e082b8dc81d8729f95bcc092db90c5.png"
                class="icon"
              />Based on <a href="https://ml5js.org/">ML5.JS</a>
              <a
                href="https://learn.ml5js.org/#/reference/sentiment?id=sentiment-movie-reviews-model-biography"
                >Image Analysis</a
              >
            </li>
            <li>
              <img
                src="https://cdn-images-1.medium.com/max/1200/1*iDQvKoz7gGHc6YXqvqWWZQ.png"
                class="icon"
              />Uses
              <a
                href="https://github.com/tensorflow/tfjs-examples/tree/482226b15a757f39871038f35b3b8aad7729e594/sentiment"
                >Tensorflow.js model</a
              >
            </li>
            <li>
              <img
                src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b2/Bootstrap_logo.svg/512px-Bootstrap_logo.svg.png"
                class="icon"
              />
              Made beautiful using
              <a href="https://getbootstrap.com/">Bootstrap</a>
            </li>
            <li>
              <img
                src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Vue.js_Logo_2.svg/555px-Vue.js_Logo_2.svg.png"
                class="icon"
              />Uses <a href="https://vuejs.org/">Vue.js</a>
            </li>
            <li>
              <img
                src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/ae/Nuxt_logo.svg/2560px-Nuxt_logo.svg.png"
                class="icon"
              />Crafted with <a href="https://nuxtjs.org/">Nuxt.js</a>
            </li>
            <li>
              <img
                src="https://pbs.twimg.com/profile_images/1366808543773384704/8qFXRmFc_400x400.png"
                class="icon"
              />Fonts by <a href="https://fonts.google.com/">Google Fonts</a>
            </li>
          </ul>
        </span>
      </div>
    </div>

    <footer class="pt-3 mt-4 text-muted border-top">
      Made by Dylan Kainth
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      prediction: null,
      error: null,
    };
  },
  watch: {},
  methods: {
    errorhandler(err) {
      this.error = err;
    },
    startVideo() {
      // Grab elements, create settings, etc.
      const video = document.getElementById("video");

      // Create a webcam capture
      navigator.mediaDevices
        .getUserMedia({ video: true })
        .then((stream) => {
          video.srcObject = stream;
          video.play();
        })
        .catch((err) => {
          this.errorhandler(err);
        });

      const loop = (classifier) => {
        classifier
          .classify()
          .then((results) => {
            this.prediction = results[0];
            loop(classifier); // Call again to create a loop
          })
          .catch((err) => {
            this.errorhandler(err);
          });
      };

      // Initialize the Image Classifier method with MobileNet passing the video as the
      // second argument and the getClassification function as the third
      ml5
        .imageClassifier("MobileNet", video)
        .then((classifier) => loop(classifier))
        .catch((err) => {
          this.errorhandler(err);
        });

      // var image = document.getElementById('image');
      // ml5.imageClassifier('MobileNet')
      // .then(classifier => classifier.classify(image))
      // .then(results => {
      //   this.prediction = (results[0])
      // });
    },
  },
  mounted() {
    this.startVideo();
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=League+Spartan:wght@300;400;500&display=swap");
* {
  font-family: "League Spartan", sans-serif;
}
.icon {
  width: 50px;
  height: 50px;
  padding-right: 10px;
  padding-left: 10px;
  object-fit: contain;
  position: relative;
}

.title {
  position: relative;
  top: 0;
  transition: top ease 0.5s;
}

.title:hover {
  top: -5px;
}

.moverbox {
  position: relative;
  top: 0;
  left:0;
  transition: top ease 0.25s;
}

.moverbox:hover {
  top: -5px;
  left: -5px;
}

.layouthorizontal{
  height:300px
}
.subtitle {
  position: relative;
  top: 0;
  transition: top ease 0.5s;
}

.subtitle:hover {
  top: 5px;
}

.icon:hover {
  animation: shake 0.5s;
}

@keyframes shake {
  0% {
    transform: translate(1px, 1px) rotate(0deg);
  }
  10% {
    transform: translate(-1px, -2px) rotate(-1deg);
  }
  20% {
    transform: translate(-3px, 0px) rotate(1deg);
  }
  30% {
    transform: translate(3px, 2px) rotate(0deg);
  }
  40% {
    transform: translate(1px, -1px) rotate(1deg);
  }
  50% {
    transform: translate(-1px, 2px) rotate(-1deg);
  }
  60% {
    transform: translate(-3px, 1px) rotate(0deg);
  }
  70% {
    transform: translate(3px, 1px) rotate(-1deg);
  }
  80% {
    transform: translate(-1px, -1px) rotate(1deg);
  }
  90% {
    transform: translate(1px, 2px) rotate(0deg);
  }
  100% {
    transform: translate(1px, -2px) rotate(-1deg);
  }
}

a {
  color: black;
  text-decoration: none;
  position: relative;
  top: 0;
  transition: top ease 0.5s;
}

a:hover {
  text-decoration: underline;
  top: -5px;
}

a:active {
  color: gray;
}

a:visited {
  color: #545454;
}

video {
  object-fit: cover;
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
}
.video-wrapper {
  position: relative;
  overflow: hidden;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
}

.fadeintoview {
  -webkit-animation: fadein 2s; /* Safari, Chrome and Opera > 12.1 */
  -moz-animation: fadein 2s; /* Firefox < 16 */
  -ms-animation: fadein 2s; /* Internet Explorer */
  -o-animation: fadein 2s; /* Opera < 12.1 */
  animation: fadein 2s;
}

@keyframes fadein {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

/* Firefox < 16 */
@-moz-keyframes fadein {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

/* Safari, Chrome and Opera > 12.1 */
@-webkit-keyframes fadein {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

/* Internet Explorer */
@-ms-keyframes fadein {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

/* Opera < 12.1 */
@-o-keyframes fadein {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
