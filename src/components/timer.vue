<template>
    <div>
        <h1 v-if="!finish">{{ parseTime }}</h1>
        <h1 v-else>Well done! Your time is {{ parseTime }}</h1>
    </div>
</template>

<script>
    export default {
        props: ['finish', 'totalSeconds'],
        created(){
            this.timer();
        },
        methods: {
            timer(){
                let interval = setInterval(()=>{
                    if(this.finish) {
                        clearInterval(interval);
                    } else {
                        this.$emit('updateTime');
                    }
                }, 1000);
            },
        },
        computed: {
            parseTime(){
                let seconds = this.totalSeconds%60 < 10 ? '0' + this.totalSeconds%60 : this.totalSeconds%60;
                let minutes = Math.floor(this.totalSeconds/60);
                return minutes + ':' + seconds;
            }
        }
    }
</script>

<style scoped>
    h1{
        text-align: center;
    }
</style>