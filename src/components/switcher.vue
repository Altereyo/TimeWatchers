<template>
    <div id="switcher">
        <h2>Welcome to the TimeWatchers!</h2>
        <h3>Here you can switch counters between timers and stopwatches.</h3>
        <div id="checkbox" @click="changeType()">
            <label for="switch">Timers</label>
            <input type="checkbox" id="switch" ref="switch">
            <label for="switch">Stopwatches</label>
        </div>
    </div>
</template>

<script>
export default {
    methods: {
        changeType() {
            if (this.$refs.switch.checked) {
                this.$parent.countersType = 'stopwatches'
            }
            else if (!this.$refs.switch.checked) {
                this.$parent.countersType = 'timers'
            }
        }
    },
}
</script>

<style lang="sass">
#switcher
    display: flex
    flex-direction: column
    align-items: center
    color: #9E9E9E

    #checkbox
        display: flex
        align-items: center

        label
            font-size: 20px
            width: 130px
            margin: 0 10px
            text-align: right

    input[type="checkbox"] 
        width: 125px
        height: 40px
        -webkit-appearance: none
        -moz-appearance: none
        background: #696969
        outline: none
        border-radius: 50px
        box-shadow: inset 0 0 5px rgba(0,0,0, .2)
        transition: 0.5s
        position: relative
        
        &::before 
            content: ''
            position: absolute
            width: 40px
            height: 40px
            border-radius: 50%
            top: 0
            left: 0
            background: #fff
            transform: scale(1.1)
            box-shadow: 0 2px 5px rgba(0,0,0, .2)
            transition: 0.5s
        
        &:checked::before 
            left: 85px
        
</style>