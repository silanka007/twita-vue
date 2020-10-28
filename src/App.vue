<template>
	<div id="app">
		<userProfile />
	</div>
</template>

<script>
import userProfile from "./components/userProfile";

export default {
	name: "App",
	components: {
		userProfile,
	},
};
</script>

<style lang="scss">
html,
body {
	height: 100%;
}

#app {
	background: rgb(250, 250, 250);
	display: flex;
	flex-direction: column;
	padding: 1rem 2rem;
	color: rgb(82, 80, 80);
	height: 100%;
}
</style>
