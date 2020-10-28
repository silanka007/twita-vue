<template>
	<div class="form-field">
		<form method="POST" @submit.prevent="onSubmitHandler">
			<div class="form-input__field">
				<label for="twit">Tweet: </label>
				<textarea name="twit" v-model="twitContent" required></textarea>
			</div>
			<div class="form-input__field">
				<select v-model="twitType">
					<option
						v-for="(type, index) in tweetTypes"
						:key="index"
						:value="type.value"
						>{{ type.name }}
					</option>
				</select>
			</div>
			<input type="submit" value="submit" />
		</form>
	</div>
</template>

<script>
export default {
	name: "AddTwitForm",
	data() {
		return {
			tweetTypes: [
				{ name: "Draft Twit", value: "draft" },
				{ name: "Instant Twit", value: "instant" },
			],
			twitType: "instant",
			twitContent: "",
		};
	},
	methods: {
		onSubmitHandler() {
            if(this.twitType === "draft"){
                return;
            }
            this.$emit("addtwit", { type: this.twitType, content: this.twitContent });
            this.twitContent = "";
		},
	},
};
</script>

<style scoped>
.form-field {
	margin-top: 1rem;
}
</style>
