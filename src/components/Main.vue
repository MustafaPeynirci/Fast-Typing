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
                <p>{{description2}}</p>
                <div>
                    <p class="display-6">Correct Word Count: {{correctWordCount}}</p>
                    <p>Correct Percentage: %{{correctPercentage}}</p>
                    <span class="badge bg-success fs-6">True: {{ trueCount }}</span>
                    <span class="badge bg-danger ms-2 fs-6">False: {{ falseCount }}</span>

                </div>
                <button class="btn btn-outline-success mt-4" type="button" @click="reloadPage">
                    Try again
                </button>
            </div>
            <div class="card border-0" v-else>
                <div class="card-body">
                    <span v-for="(word,key) in wordList.filter((data,index)=>index<15)" :key="key"
                        v-bind:class="key!=0 || writingWordControl" class="ms-1 p-2 fs-4">
                        {{ word }}
                    </span>
                </div>
                <div class="input-group input-group-lg mt-4">
                    <input type="text" class="form-control" v-model="writingWord">
                    <button class="btn btn-outline-secondary" type="button" disabled>{{timer}} sec.</button>
                </div>
            </div>
        </div>
    </div>
</template>
//test
<script>
    import wordList from '@/assets/words.json'
    export default {
        data() {
            return {
                description: "It is an easy and fun 10 finger keyboard training site. When you complete the lessons and exercises as described, you will be able to type with 10 fingers without looking at the keyboard.",
                description2:"Congratulations! The correct and incorrect information is as follows.",
                writingWord: null,
                isTrue: true,
                trueCount: 0,
                falseCount: 0,
                timer: 60,
                interval: false,
                isRunning: false,
                isFinish: false,
                wordList: wordList
            }
        },
        watch: {
            writingWord(value) {
                if (!value || value === ' ') {
                    this.writingWord = ''
                    return
                }
                 if (this.correctPercentage === NaN) {
                    this.correctPercentage = 0
                    return
                }
                if (!this.isRunning) this.toggleTimer()
                const word = this.wordList[0].slice(0, value.length).toUpperCase()
                const userWord = value.replace(" ", "").toUpperCase()

                userWord === word ? this.isTrue = true : this.isTrue = false;

                if (value.indexOf(' ') !== -1) {
                    this.isTrue ? this.trueCount++ : this.falseCount++;
                    this.wordList.shift();
                    this.writingWord = '';
                    this.isTrue = true;
                }
            }
        },
        mounted() {
            this.getWords()
        },
        computed: {
            writingWordControl() {
                return this.isTrue ? 'writing-word' : 'writing-word bg-danger';
            },
            correctWordCount() {
                return 300 - this.wordList.length
            },
            correctPercentage() {
                const percent = (100 / this.correctWordCount)
                const val = (percent * this.trueCount).toFixed(2)
                return isNaN(val) ? 0 : val
            }
        },
        methods: {
            getWords() {
                this.wordList = this.wordList.sort(() => Math.random() - 0.5).splice(0, 300)
            },
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
            },
            reloadPage() {
                window.location.reload()
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