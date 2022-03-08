<template>
  <div class="container py-4">
    <header class="pb-3 mb-4 border-bottom">
      <a
        href="/"
        class="d-flex align-items-center text-dark text-decoration-none"
      >
        
        <span class="fs-4">ML5js Sentiment Analysis Demo</span>
      </a>
    </header>

    <div class="p-5 my-4 bg-light rounded-3 shadow">
      <div class="container-fluid py-5">
        <h1 class="display-5 fw-bold title">Sentiment Analysis</h1>
        <p class="col-md-8 fs-4 subtitle">
          Enter text below and ML5.js will be used in your browser to guess the
          sentiment your input, in real time.
        </p>
      </div>
    </div>

    <div class="row align-items-md-stretch">
      <div class="col-md-6">
        <div class="h-100 p-5 bg-light rounded-3 shadow">
          <textarea 
            class="form-control form-control-lg h-100"
            :placeholder="words[i]"
            aria-label=".form-control-lg example"
            v-model="text"
            
          />
        </div>
      </div>
      <div class="col-md-6">
        <div class="h-100 p-5 bg-light rounded-3 shadow">
          <h2>Score</h2>
          <h1>{{ Math.round(prediction.score * 100) }}%</h1>

          <div class="progress">
            <div
              class="progress-bar"
              role="progressbar"
              :style="
                'width:' + Math.round(prediction.score * 100).toString() + '%'
              "
              aria-valuemin="0"
              :aria-valuenow="Math.round(prediction.score * 100)"
              aria-valuemax="100"
            ></div>
          </div>
        </div>
      </div>
    </div>

    <div class="p-5 my-4 bg-light rounded-3 shadow">
      <div class="container-fluid py-5">
        <h1 class="display-6 fw-bold">Details</h1>
        <p class="col-md-8 fs-5">
          <ul style="list-style: none">
            <li><img src="https://ml5js.org/static/ml5_logo_purple-88e082b8dc81d8729f95bcc092db90c5.png" class="icon">Based on <a href="https://ml5js.org/">ML5.JS</a> <a href="https://learn.ml5js.org/#/reference/sentiment?id=sentiment-movie-reviews-model-biography">Sentiment Analysis</a></li>
            <li><img src="https://cdn-images-1.medium.com/max/1200/1*iDQvKoz7gGHc6YXqvqWWZQ.png" class="icon">Uses <a href="https://github.com/tensorflow/tfjs-examples/tree/482226b15a757f39871038f35b3b8aad7729e594/sentiment">Tensorflow.js model</a> from <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/69/IMDB_Logo_2016.svg/2560px-IMDB_Logo_2016.svg.png" class="icon"> <a href="imdb.com">IMDB</a></li>
            <li><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b2/Bootstrap_logo.svg/512px-Bootstrap_logo.svg.png" class="icon"> Made beautiful using <a href="https://getbootstrap.com/">Bootstrap</a></li>
            <li><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Vue.js_Logo_2.svg/555px-Vue.js_Logo_2.svg.png" class="icon">Uses <a href="https://vuejs.org/">Vue.js</a></li>
            <li><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/ae/Nuxt_logo.svg/2560px-Nuxt_logo.svg.png" class="icon">Crafted with <a href="https://nuxtjs.org/">Nuxt.js</a></li>
            <li><img src="https://pbs.twimg.com/profile_images/1366808543773384704/8qFXRmFc_400x400.png" class="icon">Fonts by <a href="https://fonts.google.com/">Google Fonts</a></li>
          </ul>
          
        </p>
      </div>
    </div>

    <footer class="pt-3 mt-4 text-muted border-top">Made by Dylan Kainth</footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      text: '',
      prediction: 0,
      sentiment: null,
      words:["I hated Squid Game!", "Back to the Future Rules!", "The Harry Potter movies suck!", "A Silent Voice is a great movie", "The Godfather is a thriller"],
      i:0
    };
  },
  watch: {
    text: {
      handler: function (newValue) {
        this.generateSentiment(newValue);
      },
    },
  },
  methods: {
    generateSentiment(newValue) {
      console.log(this.sentiment.predict(newValue));
      this.prediction = this.sentiment.predict(newValue);
      console.log(this.prediction);
    },
    updateword(){
      this.word = words[Math.floor(Math.random() * words.length)];
    }
  },
  mounted() {
    // Create a new Sentiment method
    this.sentiment = ml5.sentiment('movieReviews');

    console.log('ml5 version:', ml5.version);

    setInterval(() => {
      if (this.i < this.words.length-1) this.i++
      else this.i = 0
    },3000)
  },
};
</script>

<style scoped>

@import url('https://fonts.googleapis.com/css2?family=League+Spartan:wght@300;400;500&display=swap');
* {
  font-family: 'League Spartan', sans-serif;
}
.icon {
  width:50px;
  height:50px;
  padding-right:10px;
  padding-left:10px;
  object-fit: contain;
  position: relative;

}

.title {
  position:relative;
  top: 0;
  transition: top ease 0.5s;
}

.title:hover {
  top: -5px;
}

.subtitle {
  position:relative;
  top: 0;
  transition: top ease 0.5s;
}

.subtitle:hover {
  top: 5px;
}

.icon:hover{
  animation: shake 0.5s;
}

@keyframes shake {
  0% { transform: translate(1px, 1px) rotate(0deg); }
  10% { transform: translate(-1px, -2px) rotate(-1deg); }
  20% { transform: translate(-3px, 0px) rotate(1deg); }
  30% { transform: translate(3px, 2px) rotate(0deg); }
  40% { transform: translate(1px, -1px) rotate(1deg); }
  50% { transform: translate(-1px, 2px) rotate(-1deg); }
  60% { transform: translate(-3px, 1px) rotate(0deg); }
  70% { transform: translate(3px, 1px) rotate(-1deg); }
  80% { transform: translate(-1px, -1px) rotate(1deg); }
  90% { transform: translate(1px, 2px) rotate(0deg); }
  100% { transform: translate(1px, -2px) rotate(-1deg); }
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
</style>