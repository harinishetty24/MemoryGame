<template>
  <div class="home">
    <h1>{{ msg }}</h1>
    <h3>Enter the number you see below</h3>
    <br>
    <div>
        <div>
            <p>Score: {{score}}</p>
            <p>Wrong: {{wrong}}/5</p>
        </div>
            <p>****************************</p>
            <p>{{displayNumber}}</p>
            <p>****************************</p>
		</div>
        <form id="checkNumber" @submit="checkNumber">
            <label for="guessedNumber">Number is: </label> 
            <input 
                pattern="[0-9]+"
                type="text"
                name="guessedNumber"
                id="guessedNumber" v-model="guessedNumber">
            <br/>
            <br/>
        </form> 
        <Modal v-if="showStartModal" @close="showStartModal = false">
            <h3 slot="header">Memory Game</h3>
            <p slot="body">The Game is quite simple: You will see a random number displayed which gets dissapeared in a second. All you need to do is to remember the number and enter in the textbox. On entering 5 incorrect entries, the game will be finished and displays the total score. Enough said Let's play!</p>
            <div slot="footer">
            <button class="modal-default-button" @click="resetGame">Start</button>
            </div>
        </Modal>
        <Modal v-if="showEndModal" @close="showEndModal = false">
            <h3 slot="header">Game Over!</h3>
            <div slot="body">
            <p>Not that easy isn't it! </p>
            <p>Your score is: {{score}}</p>
            </div>
            <div slot="footer">
            <button class="modal-default-button" @click="resetGame">Reset</button>
            </div>
        </Modal>
  </div>  
</template>

<script>
import Modal from './Modal.vue'

export default {
  name: 'Home',
  components: {
    Modal
  },
  props: {
    msg: String
  },
  data: function() {
    return {
      guessedNumber: '',
      displayNumber: '-----',
      generatedNumber:'',
      wrong:0,
      score:0,
      showStartModal: true,      
    };
  },
  methods:{
    checkNumber: function (e) {
      if(this.guessedNumber == this.generatedNumber) {
        this.score++;
      } else {
        this.wrong++;
      }
      this.guessedNumber = '';
      if(this.wrong >= 5) {
        this.showEndModal = true;
      }
      this.setNewNumber();
      e.preventDefault();
    },
    setNewNumber: function () {
      this.generatedNumber = Math.floor(Math.random() * (9989)) + 11;
      this.displayNumber = this.generatedNumber;
          setTimeout(function () {
            this.displayNumber = '-----';
          }.bind(this), 800);
    },
    resetGame: function () {
      this.score = 0;
      this.wrong = 0;
      this.guessedNumber = '';
      this.setNewNumber();
      this.showStartModal = false;
      this.showEndModal = false;
    }
  }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
