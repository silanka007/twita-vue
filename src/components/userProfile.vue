<template>
	<div class="user-profile">
		<div class="user-profile__user-panel">
			<h1 class="user-profile__username">@{{ user.username }}</h1>
			<div
				class="user-profile__admin-badge"
				v-if="user.isAdmin"
			>
				Admin
			</div>
			<div class="user-profile__follower-count">
				<strong>follower: {{ followers }} - </strong>
				<button v-on:click="followUser">{{ templateData.follow }}</button>
			</div>
            <AddTwitForm @addtwit="addTwit" />
		</div>
		<div class="user-profile__twits" @dragOver="dragOver">
			<Twit
				v-for="(twit, index) in user.twits"
				:key="twit.id"
				:twit="twit"
				:username="user.username"
                :data-twitid="index"
			/>
		</div>
	</div>
</template>

<script>
import Twit from "./twit";
import AddTwitForm from "./addTwit";

export default {
	name: "userProfile",
	components: {
        Twit,
        AddTwitForm
	},
	data() {
		return {
			followers: 5,
			user: {
				id: 1,
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
            dragProps: null
		};
	},
	computed: {
		fullname() {
			return `${this.user.firstName} ${this.user.lastName}`;
		},
	},
	methods: {
        // follow user by clicking follow button
		followUser() {
			this.followers++;
			this.templateData.follow = "unfollow";
        },
        // add twit to users twit
        addTwit(twitObject){
            const {content} = twitObject;
            this.user.twits.unshift({id: content, content})
        },
        // handles dragover functionality
		dragOver(e) { // this does not affect the twits array indexing
            e.preventDefault();
            const container = document.querySelector('.user-profile__twits');
            const dragOverElement = this.getClosestNode(container, e.clientY);
            const dragItem = container.querySelector('.dragging');
            container.insertBefore(dragItem, dragOverElement);
            this.dragProps = {dragItem, dragOverElement};
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
        },
       /*  updateTwitAfterDrag(dragId, dragOverId, twitArray){
            const startIndex = dragOverId < 0 ? twitArray.length + dragId : 0;
            const sourceItem = twitArray.splice(dragId,1)[0];
            twitArray.splice(startIndex, 0, sourceItem);
            return twitArray;
        },
        dragEventEnded(){
            if(this.dragProps){
                const {dragItem, dragOverElement} = this.dragProps;
                console.log(dragItem);
                console.log(dragItem.dataset.twitid, dragOverElement.dataset.twitid)
                const newArray = this.updateTwitAfterDrag(dragItem.dataset.twitid, dragOverElement.dataset.twitid, this.user.twits);
                this.user.twits = newArray;	
            }
        } */
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
