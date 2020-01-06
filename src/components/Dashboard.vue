<template>
    <div style="text-align: center; overflow: auto;">
        <img :src="avatar" class="avatar">
        <div class="nickname">Hello, {{givenName}}!</div>
        <div class="text">You Have Dedicate <span style="color: brown;">{{seconds}}</span> s.</div>
        <div class="login-button" @click.prevent="signOut">Logout and +1s</div>
    </div>
</template>

<script>
import { userSession } from '../userSession'
import { Person } from 'blockstack'

export default {
    methods: {
        signOut () {
            userSession.signUserOut(window.location.href)
        }
    },
    data () {
        return {
            avatar: 'https://s3.amazonaws.com/onename/avatar-placeholder.png',
            givenName: 'Anonymous',
            seconds: 1
        }
    },
    mounted () {
        if (userSession.isUserSignedIn()) {
            const profile = userSession.loadUserData().profile
            const user = new Person(profile)
            this.givenName = user.name() ? user.name() : 'Nameless Person'
            if (user.avatarUrl()) this.avatar = user.avatarUrl()
        } else if (userSession.isSignInPending()) {
            userSession.handlePendingSignIn()
                .then((userData) => {
                    window.location = window.location.origin
                })
        }
        if (localStorage.seconds) {
            this.seconds = localStorage.seconds
        }
        setInterval(() => {
            this.seconds++
            localStorage.seconds = this.seconds
        }, 1000)
    }
}
</script>

<style scoped>
    .avatar {
        width: 200px;
        height: 200px;
        border-radius: 100%;
        display: block;
        margin: 70px auto 0;
    }

    .nickname {
        font-size: 42px;
        margin: 40px 0;
        font-weight: bolder;
        color: #091D38;
    }

    .text {
        font-size: 36px;
        margin: 80px 0;
        font-weight: bolder;
        color: #091D38;
    }
</style>
