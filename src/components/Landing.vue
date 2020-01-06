<template>
    <div style="text-align: center">
        <div class="title">Hello, Hath!</div>
        <img class="poster" src="../../assets/image/login.jpg">
        <div class="sub">Big Brother Is Watching You...</div>
        <div class="login-button" :class="{'button-hover': preventSignIn}" @click="signIn">Sign In with Blockstack and +1s</div>
    </div>
</template>

<script>
import { userSession } from '../userSession'

export default {
    name: 'signin',
    methods: {
        signIn () {
            if (this.preventSignIn) return
            this.preventSignIn = true
            userSession.redirectToSignIn()
        }
    },
    data () {
        return {
            preventSignIn: false
        }
    }
}
</script>

<style scoped>
    .title {
        font-size: 52px;
        padding: 80px 0 50px;
        font-weight: bolder;
        color: #091D38;
    }

    .sub {
        font-size: 22px;
        padding: 5px 0 35px;
        font-weight: bolder;
        color: #091D38;
    }

    .poster {
        width: 350px;
        margin: 0 auto;
        display: block;
    }
</style>
