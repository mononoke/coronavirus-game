<template>
    <button class="button" v-on="$listeners">{{ text }}</button>
</template>

<script>
export default {
    props: {
        text: {
            type: String,
            default: ""
        }
    }
}
</script>

<style lang="stylus" scoped>
.button
    display inline-block
    white-space nowrap
    border-radius .25rem
    padding .375rem .75rem
    font-size 14px
    cursor pointer
    background-image none
    text-align center
    vertical-align middle
    line-height 1.5
    font-weight 400
    border-width 1px
    border-style solid
    box-sizing border-box
    background-color #80a3a7
    user-select none
    outline none
    transition color .15s ease-in-out,background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out
    &:hover
        background-color #3f5858
        color #ffffff
</style>
