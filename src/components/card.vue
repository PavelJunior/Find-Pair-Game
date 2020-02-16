<template>
    <transition name="slide" mode="out-in" @after-enter="afterEnter">
        <div class="box backside" @click="openItem" v-if="!open" key="1"></div>
        <div class="box frontside" v-else key="2">
            <h2>{{ number }}</h2>
            <img :src="'src/images/' + suit" height="100" width="100" rel="preload">
        </div>
    </transition>
</template>

<script>
    export default {
        props: ['card', 'open'],
        data() {
            return {

            }
        },
        computed: {
            suit(){
                let suit = this.card.charAt(0);
                if(suit == 's'){
                    return "spades.png"
                } else if (suit == 'd') {
                    return "diamonds.png"
                } else if (suit == 'c') {
                    return "clubs.png"
                } else if (suit == 'h') {
                    return "hearts.png"
                }
            },
            number(){
                let number = this.card.substr(1);
                return number;
            }
        },
        methods: {
            openItem(){
                this.$emit('opened');
            },
            afterEnter(){
                this.$emit('afterEnterDone')
            }
        }
    }
</script>

<style scoped>

</style>