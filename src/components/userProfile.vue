<template>
    <div class="user-profile">
        <div class="user-profile__user-panel">
            <h1 class="user-profile__username">@{{user.username}} </h1>
            <div class="user-profile__admin-badge" v-if="user.isAdmin && followers > 10">Admin</div>
            <div class="user-profile__follower-count">
                <strong>follower: {{ followers }} - </strong>
                <button v-on:click="followUser" >Follow</button>
            </div>
        </div>
    </div>

    <div class="user-profile" v-for="twit in user.twits" :key="twit.id">
        <div>{{twit.content}}</div>
    </div>
</template>

<script>
export default {
    name: "userProfile",

    data(){
        return {
            followers: 0,
            user: {
                id: 1,
				followers: 0,
				username: "Rx_Silanka",
				firstName: "Paul",
				lastName: "Onyekwelu",
				email: "silanka007@gmail.com",
                isAdmin: true,
                twits: [
                    {id: 1, content: "twitter is fun!"},
                    {id: 2, content: "same is vue js"},
                    {id: 3, content: " I prefer reactJs though :)"},
                ]
            }
        }
    },
    computed: {
        fullname() {
            return `${this.user.firstName} ${this.user.lastName}`;
        }
    },
    methods: {
        followUser() {
            this.followers++
        }
    }
}
</script>

<style scoped>
.user-profile{
    background: white;
    padding: 1rem;
    box-shadow: 0px 0px 4px rgb(189, 189, 189);
    width: 80vw;
    max-width: 400px;
}

.user-profile__user-panel{
    display: flex;
    flex-direction: column;
}

.user-profile__admin-badge{
    background: rebeccapurple;
    color: white;
    padding: 3px;
    margin-right: auto;
    border-radius: 5px;
}

</style>