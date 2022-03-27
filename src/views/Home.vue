<template>
  <div class="advice">
    <h4 class="advice-nbr">Advice #117</h4>
    <div class="advice-text">
      
      <section v-if="errored">
        <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
      </section>
      
      <section v-else>
        <p v-if="loading">Loading...</p>
        <p v-else>
          {{ advices.advice }}
        </p>
      </section>
    </div>
    
    <span class="divider">
      <i class="fa-solid fa-grip-lines-vertical"></i>
    </span>
    
    <button class="dice-btn" @click="loadAdvice()">
      <i class="fa-solid fa-dice-five"></i>
    </button>
  </div>

  
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      advices: '',
      loading: true,
      errored: false,
    }
  },
  beforeMount() {
    this.loadAdvice()
  },
  methods: {
    // reloadPage() {
    //   window.location.reload();
    // },
    loadAdvice() {
      axios
      .get('https://api.adviceslip.com/advice')
      .then(response => (this.advices = response.data.slip))
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
    }
  }
}
</script>

<style lang="scss">
  .advice {
    background-color: hsl(217, 19%, 24%);
    width: 600px;
    max-width: 100%;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    border-radius: 10px;
    padding: 40px;
    box-sizing: border-box;
    text-align: center;
    color: hsl(193, 38%, 86%);

    .advice-nbr {
      font-size: 14px;
      text-transform: uppercase;
      letter-spacing: 2px;
      color: hsl(150, 100%, 66%);
      line-height: 1.5em;
      margin-bottom: 20px;
    }

    .advice-text {
      font-size: 24px;
      line-height: 1.4em;
      margin-bottom: 40px;
    }

    .divider {
      display: block;
      content: '';
      width: 100%;
      height: 1px;
      background: hsl(217, 19%, 38%);
      position: relative;
      margin-bottom: 40px;

      i {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background: hsl(217, 19%, 24%);
        padding: 0 10px;
        color: hsl(193, 38%, 86%);
        font-size: 24px;
      }
    }

    .dice-btn {
      display: inline-block;
      background-color: hsl(150, 100%, 66%);
      outline: none;
      border: none;
      cursor: pointer;
      width: 56px;
      height: 56px;
      border-radius: 50%;
      position: absolute;
      bottom: -24px;
      left: 50%;
      transform: translateX(-50%);
      transition: .5s all ease-in-out;

      i {
        font-size: 24px;
        color: hsl(218, 23%, 16%);
      }

      @keyframes mymove {
        50% {transform: rotate(180deg);}
      }

      &:hover {
        background-color: hsl(193, 38%, 86%);
        
        i {
          animation: mymove .5s;
        }
      }
    }
  }
</style>
