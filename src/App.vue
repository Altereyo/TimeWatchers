<template>
    <div id="app">
        <switcher />
        <div id="counters-wrapper">
            <counter v-for="counter of countersDB" :counter="counter" :countersType="countersType" :key="counter.id" />
            <add-counter :countersType="countersType" />
        </div>
    </div>
</template>

<script>
import switcher from "./components/switcher.vue"
import counter from "./components/counter.vue"
import addCounter from "./components/add-counter.vue"

export default {
    components: { switcher, counter, addCounter },
    data() {
        return {
            countersType: 'timers',
            countersDB: [],
            countersAPI: './db/counters.json'
        }
    },
    methods: {
        getCounters(api) {
            return fetch(api)
            .then(d => d.json())
            .then(d => this.countersDB = d)
            .then(d => console.log(this.countersDB))
        }
    },
    mounted() {
        this.getCounters(this.countersAPI)
    },
}
</script>

<style lang="sass">
*
    box-sizing: border-box
    font-family: Montserrat
    
body
    background-color: #353638

#counters-wrapper
    @media screen and (min-width: 320px)
        width: 225px
    @media screen and (min-width: 768px)
        width: 500px
    @media screen and (min-width: 1024px)
        width: 775px

    padding-top: 50px
    margin: 0 auto
    display: flex
    flex-wrap: wrap
    justify-content: flex-start


</style>
