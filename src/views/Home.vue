<template>
  <div class="advice">
    <div class="advice-text">
      <section v-if="errored">
        <p>
          We're sorry, we're not able to retrieve this information 
          at the moment, please try back later
        </p>
      </section>
      
      <section v-else>
        <p v-if="loading">Loading...</p>
        <p v-else>
          <!-- {{ advices.advice }} -->
          {{ singleAdvice.text }}
        </p>
      </section>
    </div>

    <div class="controls-btn">
      <button class="prev-btn" @click="prevAdvice()" :disabled="isDisabledPrev">
        <i class="fa-solid fa-left-long"></i>
      </button>
      <button class="next-btn" @click="nextAdvice()" :disabled="isDisabledNext">
        <i class="fa-solid fa-right-long"></i>
      </button>
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
      singleAdvice: '',
      isDisabledNext: false,
      isDisabledPrev: false,
      
      myArray: [
        {
          id: null,
          text: ''
        }
      ],
      
    }
  },
  beforeMount() {
    this.loadAdvice()
  },
  methods: {
    loadAdvice() {
      axios
      .get('https://api.adviceslip.com/advice')
      .then(response => {
        (this.advices = response.data.slip)

        this.myArray.push({id: this.advices.id, text: this.advices.advice})
        this.singleAdvice = this.myArray[this.myArray.length - 1]

        this.isDisabledNext = true
        if( this.myArray.length <= 2 ) {
          this.isDisabledPrev = true
        } else {
          this.isDisabledPrev = false
        }
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
    },
    prevAdvice() {
      let indx = this.myArray.indexOf(this.singleAdvice)
      let value = this.myArray[indx - 1]
      this.singleAdvice = value

      if ( indx - 1 === 1 ) {
        this.isDisabledPrev = true
      }
      
      let lastItem = this.myArray[this.myArray.length - 1]
      if ( indx + 1 !== this.myArray.indexOf(lastItem) ) {
        this.isDisabledNext = false
      }
    },
    nextAdvice() {
      let indx = this.myArray.indexOf(this.singleAdvice)
      let value = this.myArray[indx + 1]
      this.singleAdvice = value
      
      let lastItem = this.myArray[this.myArray.length - 1]
      if ( indx + 1 === this.myArray.indexOf(lastItem) ) {
        this.isDisabledNext = true
      }

      if ( indx - 1 !== 1 ) {
        this.isDisabledPrev = false
      }
    }
  }
}
</script>

<style lang="scss">
  .advice {
    background-color: hsl(217, 19%, 24%);
    width: 700px;
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
      padding: 0 30px;
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

    .controls-btn {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 100%;
      display: flex;
      justify-content: space-between;
      padding-bottom: 20px;
    }

    .prev-btn {
      margin-left: 34px;
      background: transparent;
      outline: none;
      border: none;
      cursor: pointer;

      i {
        font-size: 20px;
        color: hsl(150, 100%, 66%);
        transition: .3s all ease-in-out;
      }

      &:hover {
        i {
          color: hsl(193, 38%, 86%);
        }
      }

      &:disabled {
        cursor: default;

        i {
          color: hsl(217, 19%, 38%);
        }
      }
    }

    .next-btn {
      margin-right: 34px;
      background: transparent;
      outline: none;
      border: none;
      cursor: pointer;

      i {
        font-size: 20px;
        color: hsl(150, 100%, 66%);
        transition: .3s all ease-in-out;
      }

      &:hover {
        i {
          color: hsl(193, 38%, 86%);
        }
      }

      &:disabled {
        cursor: default;
        
        i {
          color: hsl(217, 19%, 38%);
        }
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
