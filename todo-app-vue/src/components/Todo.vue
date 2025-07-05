<template>
    <div>
        <h1>Hello there</h1>
        <p :style="isAboveFive ? 'color: red' : 'color: blue'">Current count: {{ count }}</p>
        <button @click="increment">Increment count</button>
        <button @click="reset">Reset</button>


        <form @submit="submitName">
            <label for="name">Please Enter your name: </label>
            <input id="name" name="name" type="text" v-model="name">
            <button type="submit" :disabled="name.trim().length === 0">Submit</button>
        </form>

        <p>{{ name }}</p>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                count: 0,
                name: 'Alex'
            }
        },
        methods: {
            increment() {
                this.count++
            },
            reset() {
                this.count = 0
            },
            submitName(e) {
                e.preventDefault()
                //post request goes here
            }
        },
        computed: {
            isAboveFive() {
                return this.count >= 5
            }
        },
        watch: {
            count(newValue, oldValue) {
                if(newValue > 10) {
                    this.$emit('count-passed-ten', oldValue)
                }
            }
        }
    }
</script>