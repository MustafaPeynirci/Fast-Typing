<template>
    <div class="container">
        <div class="jumbotron mt-5">
            <h1 class="display-4">Fast Typing Test</h1>
            <p class="lead">{{description}}</p>
            <div>
                <span class="badge bg-success fs-6">True: {{ trueCount }}</span>
                <span class="badge bg-danger ms-2 fs-6">False: {{ falseCount }}</span>
            </div>
            <hr class="my-4">
            <span v-for="(word,key) in words" :key="key" v-bind:class="key!=0 || writingWordControl"
                class="ms-1 p-2 fs-4">{{ word }}</span>
            <div class="input-group input-group-lg mt-4">
                <input type="text" class="form-control" v-model="writingWord">
                <button class="btn btn-outline-secondary" type="button" disabled>1:00</button>
                <button class="btn btn-primary" type="button">Refresh</button>
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
                interval: false
            }
        },
        watch: {
            writingWord(value) {
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
        methods: {}
    }
</script>

<style>
    .writing-word {
        border-radius: 5px;
        background-color: grey;
        color: #fff;
    }
</style>