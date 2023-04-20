<template>
    <div class="container">
        <div class="jumbotron mt-5">
            <h1 class="display-4">Fast Typing</h1>
            <p class="lead">{{description}}</p>
            <div v-if="!isFinish">
                <span class="fs-6">True: {{ trueCount }}</span>
                <span class="ms-2 fs-6">False: {{ falseCount }}</span>
            </div>
            <hr class="my-4">
            <div class="alert alert-success" v-if="isFinish">
                <h4>Finish</h4>
                <p>Congratulations! The correct and incorrect information is as follows.</p>
                <div>
                    <span class="badge bg-success fs-6">True: {{ trueCount }}</span>
                    <span class="badge bg-danger ms-2 fs-6">False: {{ falseCount }}</span>

                </div>
                    <button class="btn btn-outline-success mt-4" type="button">Try again</button>
            </div>
            <div v-else>
                <span v-for="(word,key) in words" :key="key" v-bind:class="key!=0 || writingWordControl"
                    class="ms-1 p-2 fs-4">
                    {{ word }}
                </span>
                <div class="input-group input-group-lg mt-4">
                    <input type="text" class="form-control" v-model="writingWord">
                    <button class="btn btn-outline-secondary" type="button" disabled>{{timer}} sec.</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                words: ["portakal", "elma", "kivi", "çilek", "muz", "armut", "kiraz", "karpuz", "kavun", "erik"],
                description: "It is an easy and fun 10 finger keyboard training site. When you complete the lessons and exercises as described, you will be able to type with 10 fingers without looking at the keyboard.",
                writingWord: null,
                isTrue: true,
                trueCount: 0,
                falseCount: 0,
                timer: 60,
                interval: false,
                isRunning: false,
                isFinish: false
            }
        },
        watch: {
            writingWord(value) {
                if (!this.isRunning) this.toggleTimer()
                const word = this.words[0].slice(0, value.length)
                const userWord = value.replace(" ", "")

                userWord === word ? this.isTrue = true : this.isTrue = false;

                if (value.indexOf(' ') !== -1) {
                    this.isTrue ? this.trueCount++ : this.falseCount++;
                    this.words.shift();
                    this.writingWord = '';
                }
            }
        },
        computed: {
            writingWordControl() {
                return this.isTrue ? 'writing-word' : 'writing-word bg-danger';
            }
        },
        methods: {
            toggleTimer() {
                this.isRunning = true
                this.interval = setInterval(this.timeProcess, 1000)
            },
            timeProcess() {
                this.timer--
                if (this.timer === 0) {
                    clearInterval(this.interval)
                    this.isFinish = true
                }
            }
        }
    }
</script>

<style>
    .writing-word {
        border-radius: 5px;
        background-color: grey;
        color: #fff;
    }
</style>