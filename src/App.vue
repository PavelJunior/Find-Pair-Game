<template>
  <div class="container">
    <timer @updateTime="time++"
           :totalSeconds="time"
           :finish="finish"
    ></timer>
    <div class="progress">
      <div class="progress-bar" :style="barProgress"></div>
    </div>

    <div class="card-container">
      <card v-for="(card, index) in cards"
            :open="card.open"
            :card="card.card"
            :key="index"
            @opened="checkOpenCard(index)"
            @afterEnterDone="afterCardEnter(index)"
      ></card>
    </div>
  </div>
</template>

<script>
  import card from './components/card';
  import timer from './components/timer';

  export default {
  name: 'app',
  data () {
    return {
      currentCard: null,
      time: 0,
      finish: false,
      hideWrong: false,
      pairs: 0,
      active: false,
      cards: [],
    }
  },
    created(){
      this.cards = [];
      let cardsSuits = ['s', 'd', 'h', 'c'];
      let usedCards = [];
      for(let i = 0; i < 9; i++){
        let card = cardsSuits[this.getRandomNumberInRange(0, 3)] + this.getRandomNumberInRange(2, 9);
        if(card in usedCards) {
          i-=1;
          continue;
        }
        usedCards[card] = true;
        let cardObjectFirst = {card, open: false, inCombo: false};
        let cardObjectSecond = {card, open: false, inCombo: false};
        this.cards.push(cardObjectFirst);
        this.cards.push(cardObjectSecond);
      }

      for(let j = 0; j < 10; j++){
        let currentIndex = this.cards.length, temporaryValue, randomIndex;
        while (0 !== currentIndex) {
          randomIndex = Math.floor(Math.random() * currentIndex);
          currentIndex -= 1;
          temporaryValue = this.cards[currentIndex];
          this.$set(this.cards, currentIndex, this.cards[randomIndex]);
          this.$set(this.cards, randomIndex, temporaryValue);
        }
      }
    },
    computed: {
      barProgress() {
        return {
          width: (this.pairs / (this.cards.length/2) * 100) + '%'
        }
      }
    },
    methods: {
      checkOpenCard(index){
        if(this.active == false){
          if(this.currentCard == null){
            this.cards[index].open = true;
            this.currentCard = index;
          } else {
            this.cards[index].open = true;
            if(this.cards[index].card ==  this.cards[this.currentCard].card) {
              this.pairs++;
              if(this.pairs == this.cards.length/2){
                this.finish = true;
              }
              this.currentCard = null;
            } else {
              this.hideWrong = true;
            }
          }
          this.active = true;
        }
      },
      afterCardEnter(index){
        if(this.active == true && this.hideWrong == false) {
          this.active = false;
        }

        if(this.hideWrong){
          this.cards[index].open = false;
          this.cards[this.currentCard].open = false;
          this.hideWrong = false;
          this.currentCard = null;
        }
      },
      getRandomNumberInRange(min, max){
        return Math.floor(Math.random() * (max-min+1) + min);
      },
    },
    components: {
      card,
      timer
    }
}
</script>