<template>
	<div class="form-field">
		<form method="POST" @submit.prevent="onSubmitHandler">
			<div class="form-input__field" >
				<label for="twit">Tweet: </label>
				<span>{{maxTwitContentLength}}/180</span>
				<textarea name="twit" v-model="twitContent" required :class="{'--exceeded': maxTwitContentLength > 180}"></textarea>
			</div><br>
			<div class="form-input__field">
				<select v-model="twitType">
					<option
						v-for="(type, index) in tweetTypes"
						:key="index"
						:value="type.value"
						>{{ type.name }}
					</option>
				</select>
			</div><br>
			<input v-if="maxTwitContentLength > 180" type="submit" value="submit" disabled />
			<input v-else type="submit" value="submit" />
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
	computed: {
		maxTwitContentLength() {
			return this.twitContent.length;
		}
	}
};
</script>

<style lang="scss" scoped>
.form-field {
	margin-top: 1rem;

	.--exceeded{
		color: red;
		border-color: red;
		outline-color: red;
	}
}
</style>
