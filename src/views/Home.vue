<template>
  <div class="advice">
    <h4 class="advice-nbr">Advice #117</h4>
    <p class="advice-text">
      <!-- Lorem ipsum dolor sit amet, consectetur adipisicing elit. 
      Cumque voluptatibus. -->
      {{ advices.advice }}
    </p>
    <span class="divider">
      <i class="fa-solid fa-grip-lines-vertical"></i>
    </span>
    <button class="dice-btn">
      <i class="fa-solid fa-dice-five"></i>
    </button>
  </div>

  <section v-if="errored">
    <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
  </section>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      advices: null,
      errored: false,
    }
  },
  mounted() {
    axios
      .get('https://api.adviceslip.com/advice')
      .then(response => (this.advices = response.data.slip))
      .catch(error => {
        console.log(error)
        this.errored = true
      })
  },
}
</script>

<style lang="scss">
  .advice {
    background-color: hsl(217, 19%, 24%);
    max-width: 600px;
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

      i {
        font-size: 24px;
        color: hsl(218, 23%, 16%);
      }
    }
  }
</style>
