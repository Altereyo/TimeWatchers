<template>
    <div class="counter" :style="pickColor(counter.status)">
        <span>{{ showTime(counter.timeShown) }}</span>
        <hr :style="pickColor(counter.status, 'hr')">
        <div class="btn-wrapper">
            <transition name="fade">
                <button v-if="counter.status == 'pause'" @click="start(counter)">
                    <svg width="17" height="20" viewBox="0 0 17 20" xmlns="http://www.w3.org/2000/svg">
                        <path d="M0 20V0L17 10L0 20Z" :style="pickColor(counter.status)"/>
                    </svg>
                </button>
                <button v-else-if="counter.status == 'active'" @click="pause(counter)">
                    <svg width="10" height="20" viewBox="0 0 10 20" xmlns="http://www.w3.org/2000/svg">
                        <rect x="7" width="3" height="20" :style="pickColor(counter.status)"/>
                        <rect width="3" height="20" :style="pickColor(counter.status)"/>
                    </svg>
                </button>
                <button v-if="counter.status == 'expired'" @click="remove(counter)">
                    <svg width="20" height="20" xmlns="http://www.w3.org/2000/svg" id="Capa_1" enable-background="new 0 0 515.556 515.556" viewBox="0 0 515.556 515.556">
                        <path d="m64.444 451.111c0 35.526 28.902 64.444 64.444 64.444h257.778c35.542 0 64.444-28.918 64.444-64.444v-322.222h-386.666z"/>
                        <path d="m322.222 32.222v-32.222h-128.889v32.222h-161.111v64.444h451.111v-64.444z"/>
                    </svg>
                </button>
            </transition>
            <button @click="reload(counter)">
                <svg width="20" height="20" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                    <rect width="20" height="20" :style="pickColor(counter.status)"/>
                </svg>
            </button>
        </div>
    </div>
</template>

<script>
export default {
    props: [ 'counter', 'countersType' ],
    data() {
        return {
            db: this.$parent.countersDB
        }
    },
    methods: {
        pickColor(status, tag = 'other') {
            let style = ''
            if (tag == 'hr') {
                switch (status) {
                case 'active':
                case 'expired':
                    style = 'border-bottom-color: #fff'
                    break
                case 'pause':
                    style = 'border-bottom-color: #9E9E9E;'
                    break
                }
            }
            else {
                switch (status) {
                    case 'active':
                    case 'expired':
                        style = 'fill: #fff; color: #fff;'
                        break
                    case 'pause':
                        style = 'fill: #9E9E9E; color: #9E9E9E;'
                        break
                }
            }
            return style
        },
        start(counter) {
            counter.status = 'active';

            let arr = counter.timeShown.toString().split(':'),
                hours = arr[0],
                minutes = arr[1],
                seconds = arr[2];

            counter.interval = setInterval(() => {
                if (this.countersType == 'timers') {
                    seconds--;
                    
                    if (seconds < 0 && minutes == 0 && hours > 0) {
                        seconds = 59;
                        minutes = 59;
                        hours--;
                    }
                    else if (seconds < 0 && minutes > 0 ){
                        seconds = 59;
                        minutes--;
                    }
                    else if (seconds == 0 && minutes == 0 && hours == 0) {
                        counter.status = 'expired';
                        counter.timeShown = 'Timer expired!';
                        clearInterval(counter.interval);
                        return;
                    }
                }
                else if (this.countersType == 'stopwatches') {
                    seconds++;
                    
                    if (seconds == 60) {
                        seconds = 0;
                        minutes++;
                    }
                    if (minutes == 60) {
                        minutes = 0;
                        hours++;
                    }
                }
                counter.timeShown = hours +':'+ minutes +':'+ seconds;
            }, 1000);
        },
        pause(counter) {
            counter.status = 'pause';
            clearInterval(counter.interval);
        },
        remove(counter) {
            this.db.splice(this.db.indexOf(counter), 1)
        },
        reload(counter) {
            counter.status = 'pause';
            clearInterval(counter.interval)
            console.log(this.countersType)
            if (this.countersType == 'timers') {
                counter.timeShown = counter.timerDefault
            }
            else if (this.countersType == 'stopwatches') {
                counter.timeShown = '0:0:0'
            }
        },
        showTime(time) {
            let arr = time.split(':')
            while (arr[0] == 0) {
                if (arr.length != 1) arr.shift()
                else break
            }
            return arr.join(':')
        },
    },
}
</script>

<style lang="sass">
.counter
    height: 120px
    width: 225px
    padding: 20px 0
    margin-bottom: 45px
    background-color: #696969
    display: flex
    flex-direction: column
    justify-content: center
    align-items: center
    overflow: hidden
    transition: .2s
            
    @media screen and (min-width: 768px) and (max-width: 1024px)
        &:not(:nth-child(2n))
            margin-right: 45px

    @media screen and (min-width: 1024px)
        &:not(:nth-child(3n))
            margin-right: 45px

    span
        font-size: 22px
        height: 22px
    hr
        width: 100%
        border: none
        border-bottom: 1px solid #fff
        margin: 20px 0

    .btn-wrapper 
        display: flex
        justify-content: center
        align-items: center

    button
        height: 20px
        width: 22px
        color: inherit
        background-color: transparent
        border: none
        outline: none
        margin: 0 25px
        padding: 0

        &:hover
            cursor: pointer

    svg
        color: inherit
        font-size: 20px

.fade-enter-active, .fade-leave-active 
    transition: opacity .5s
    
.fade-enter, .fade-leave-to
    opacity: 0
</style>