<template>
    <div
        class="covid"
        :style="{
            'grid-template-columns': 'repeat(' + (round * 2 + 2) + ' , 50px',
            'grid-template-rows': 'repeat(' + (round * 2 + 2) + ' , 50px'
        }"
    >
        <div
            v-for="(item, index) in newItems"
            :key="index"
            class="covid__item"
            :class="{
                'covid__item_training': item.training,
                'covid__item_killed': item.killed,
            }"
            @click="kill(item)"
        >
            <span v-if="!item.killed">{{ item.name }}</span>
        </div>
    </div>
</template>

<script>

export default {
    name: 'Covid',
    data: () => {
        return {
            round: 0,
            sum: 0,
            newItems: [],
            selectedItem: {}
        }
    },
    computed: {
        itemsNumber() {
            return (2 + this.round * 2) * (2 + this.round * 2)
        },
        items() {
            let items = []
            for (let i = 0; i < this.itemsNumber / 2; i++) {
                items.push(
                    {
                        id: Math.random().toString(36).substring(7),
                        name: i,
                        killed: false,
                        training: false
                    },
                    {
                        id: Math.random().toString(36).substring(7),
                        name: i,
                        killed: false,
                        training: false
                    }
                )
            }
            let newItems = []
            for (let i = 0; i < this.itemsNumber; i++) {
                const random = Math.floor(Math.random() * (items.length - 1))
                newItems.push(items[random])
                items.splice(random, 1)
            }
            return newItems
        }
    },
    watch: {
        items: {
            immediate: true,
            deep: true,
            handler(to) {
                this.newItems = to.map((item, index) => Object.assign({
                    index
                }, item))
            }
        },
        sum(to) {
            if (to === this.items.length / 2) {
                this.$emit('changeRound')
                this.sum = 0
                this.round++
            }
        }
    },
    methods: {
        kill(item) {
            if (item.training || item.disabled) {
                return
            }

            if (!this.selectedItem.id) {
                this.selectedItem = item
                this.newItems[item.index].training = true
                return
            }

            if (item.name !== this.selectedItem.name) {
                this.newItems[this.selectedItem.index].training = false
                this.selectedItem = item
                this.newItems[item.index].training = true
                return
            }

            this.newItems[item.index].training = true
            this.newItems[item.index].killed = true
            this.newItems[this.selectedItem.index].killed = true
            this.selectedItem = {}
            this.sum++
        }
    }
}
</script>

<style lang="stylus" scoped>
.covid
    display grid

.covid__item
    font-weight 600
    line-height 50px
    color #000000
    font-size 16px
    text-align center
    background-image none
    cursor pointer
    &.covid__item_training
        color #ffffff
        background-size cover
        background-position center
        background-image url('../assets/black_fur.png')
        cursor default
    &.covid__item_killed
        background-image url('../assets/covid.png')
        font-size 12px
        cursor default
</style>
