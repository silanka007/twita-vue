<template>
	<div class="user-profile">
		<div class="user-profile__user-panel">
			<h1 class="user-profile__username">@{{ user.username }}</h1>
			<div
				class="user-profile__admin-badge"
				v-if="user.isAdmin && followers > 10"
			>
				Admin
			</div>
			<div class="user-profile__follower-count">
				<strong>follower: {{ followers }} - </strong>
				<button v-on:click="followUser">{{ templateData.follow }}</button>
			</div>
		</div>
		<div class="user-profile__twits" @dragover="dragOver">
			<Twit
				v-for="twit in user.twits"
				:key="twit.id"
				:twit="twit"
				:username="user.username"
			/>
		</div>
	</div>
</template>

<script>
import Twit from "./twit";
export default {
	name: "userProfile",
	components: {
		Twit,
	},
	data() {
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
					{ id: 1, content: "twitter is fun!" },
					{ id: 2, content: "same is vue js" },
					{ id: 3, content: " I prefer reactJs though :)" },
				],
			},
			templateData: {
				follow: "follow",
			},
		};
	},
	computed: {
		fullname() {
			return `${this.user.firstName} ${this.user.lastName}`;
		},
	},
	methods: {
		followUser() {
			this.followers++;
			this.templateData.follow = "unfollow";
		},
		dragOver(e) {
            e.preventDefault();
            // const container = e.target.parentNode;
            const container = document.querySelector('.user-profile__twits')
            const dragOverElement = this.getClosestNode(container, e.clientY);
            const dragItem = container.querySelector('.dragging');
            container.insertBefore(dragItem, dragOverElement)	
        },
        getClosestNode(container, y){
            const draggableElement = [...container.querySelectorAll('.draggable:not(.dragging)')];

            return draggableElement.reduce((closest, child) => {
                const box = child.getBoundingClientRect();
                const offset = y - (box.top + (box.height/2));
                if(offset < 0 && offset > closest.offset){
                    return {element: child, offset};
                }else{
                    return closest;
                }
            }, {offset: Number.NEGATIVE_INFINITY}).element;
        }
	},
};
</script>

<style scoped>
.user-profile {
	display: flex;
	justify-content: space-between;
}

.user-profile__user-panel {
	display: flex;
	flex-direction: column;
	background: white;
	padding: 1rem;
	box-shadow: 0px 0px 4px rgb(189, 189, 189);
	margin-bottom: auto;
	margin-right: 1rem;
	flex: 1;
}

.user-profile__admin-badge {
	background: rebeccapurple;
	color: white;
	padding: 3px;
	margin-right: auto;
	border-radius: 5px;
}

.user-profile__twits {
	flex: 2;
    padding: 0 1rem;
}
</style>
