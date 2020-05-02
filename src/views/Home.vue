<template>
    <div
        class="home"
        :class="{'home__earth home__image': earth}"
    >
        <Covid v-if="!earth" @changeRound="changeRound()"/>
        <div
            v-if="earth"
            class="home__covid home__image"
            :style="{height: covidHeight + 'px', width: covidHeight + 'px'}"
        ></div>
        <div class="home__back">
            <div v-for="round in rounds" :key="round" class="home__round home__image" />
        </div>
    </div>
</template>

<script>
import Covid from '@/components/Covid'

export default {
    name: 'Home',
    components: {
        Covid
    },
    data: () => {
        return {
            rounds: [],
            earth: false,
            covidHeight: 10
        }
    },
    methods: {
        changeRound() {
            this.rounds.push(this.rounds.length)
            if (this.rounds.length === 1) {
                this.rounds = []
                this.earth = true
                this.growCovid()
            }
        },
        growCovid() {
            setTimeout(() => {
                this.covidHeight = this.$el.clientHeight * 3
            }, 500)
        }
    },
    beforeDestroy() {
        this.stopGrow()
    }
}
</script>

<style lang="stylus" scoped>
.home
    display flex
    align-items center
    justify-content center
    height 100vh
    width 100%
    padding 15px
    box-sizing border-box
    overflow hidden
    &.home__earth
        background-image url('../assets/earth.png')

.home__back
    position absolute
    top 0
    height 100%
    width 100%
    display grid
    grid-template-columns 1fr 1fr
    grid-template-rows 1fr 1fr
    z-index -1

.home__round
    background-image url('../assets/covid.png')

.home__image
    background-size contain
    background-repeat no-repeat
    background-position center

.home__covid
    background-image url('../assets/covid.png')
    transition all linear 20s
    z-index 3
</style>
