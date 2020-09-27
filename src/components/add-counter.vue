<template>
    <div class="counter add-timer" style="color: #fff">
        <div v-if="addingNewTimer" class=" add-wrapper">
            <div class="inputs">
                <input type="number" class="timeInput" min="0" max="99" value="0" v-model="hours" ref="hours" name="hours">
                <span>:</span>
                <input type="number" class="timeInput" min="0" max="60" value="0" v-model="minutes" ref="minutes" name="minutes">
                <span>:</span>
                <input type="number" class="timeInput" min="0" max="60" value="0" v-model="seconds" ref="seconds" name="seconds">
            </div>
            <hr>
            <button @click="addTimer()">
                <font-awesome-icon icon="check"/>
            </button>
        </div>
        <div v-if="!addingNewTimer" class="add-wrapper">
            <div class="btn-wrapper">
                <button @click="clickPlus()">
                    <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <rect x="8.5" width="3" height="20" fill="#9E9E9E"/>
                        <rect y="11.5" width="3" height="20" transform="rotate(-90 0 11.5)" fill="#9E9E9E"/>
                    </svg>
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['countersType'],
    data() {
        return {
            addingNewTimer: false,
            hours: '0',
            minutes: '0',
            seconds: '0'
        }
    },
    methods: {
        clickPlus() {
            if (this.countersType == 'stopwatches') {
                this.addCounter()
            }
            else if (this.countersType == 'timers') {
                this.addingNewTimer = true
            }
        },
        addTimer() {
            // сначала валидируем поля
            // часы
            if (this.hours < 0 || this.hours == '') {
                this.$refs.hours.style.borderBottomColor = 'red';
                return
            } else {
                this.$refs.hours.style.borderBottomColor = '#9E9E9E'
            }
            // минуты
            if (this.minutes < 0 || this.minutes == '') {
                this.$refs.minutes.style.borderBottomColor = 'red';
                return
            } else {
                this.$refs.minutes.style.borderBottomColor = '#9E9E9E'
            }
            // секунды
            if (this.seconds < 0 || this.seconds == '') {
                this.$refs.seconds.style.borderBottomColor = 'red';
                return
            } else {
                this.$refs.seconds.style.borderBottomColor = '#9E9E9E'
            }
            // всё вместе
            if (this.seconds == '0' && this.minutes == '0' && this.hours == '0') {
                this.$refs.seconds.style.borderBottomColor = 'red';
                this.$refs.minutes.style.borderBottomColor = 'red';
                this.$refs.hours.style.borderBottomColor = 'red';
                return
            } else {
                this.$refs.seconds.style.borderBottomColor = '#9E9E9E';
                this.$refs.minutes.style.borderBottomColor = '#9E9E9E';
                this.$refs.hours.style.borderBottomColor = '#9E9E9E';
            }
            // теперь добавляем таймер
            this.addCounter()
        },
        addCounter() {
            // делаем новый id и объект со счётчиком
            let newID = this.$parent.countersDB.length + 1;
            let newCounter = {
                "id": newID,
                "interval": "",
                "status": "pause",
                "timerDefault": this.hours +':'+ this.minutes +':'+ this.seconds,
                "timeShown": this.hours +':'+ this.minutes +':'+ this.seconds
            }
            // добавляем его в БД и прибираемся
            this.$parent.countersDB.push(newCounter);
            this.hours = '0';
            this.minutes = '0';
            this.seconds = '0';
            this.addingNewTimer = false;
        }
    },
}
</script>

<style lang="sass">
.add-timer
    padding: 15px 0

    .add-wrapper
        width: 100%
        display: flex
        flex-direction: column
        justify-content: center
        align-items: center
    
    hr
        color: #fff

    .timeInput
        width: 40px
        display: inline-block
        outline: none
        background-color: transparent
        border: none
        border-bottom: 1px solid #9E9E9E
        color: #fff
        padding: 0
        text-align: center
        
        &, span
            font-family: Montserrat
            font-size: 22px
            height: 22px

        &::-webkit-outer-spin-button,
        &::-webkit-inner-spin-button 
            -webkit-appearance: none
            margin: 0

</style>