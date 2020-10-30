<template>
	<div class="form-field">
		<form method="POST" @submit.prevent="onSubmitHandler">
			<div class="form-input__field">
				<label for="twit">Tweet: </label>
				<span>{{ maxTwitContentLength }}/180</span>
				<textarea
					name="twit"
					v-model="state.twitContent"
					required
					:class="{ '--exceeded': maxTwitContentLength > 180 }"
				></textarea>
			</div>
			<br />
			<div class="form-input__field">
				<select v-model="state.twitType">
					<option
						v-for="(type, index) in state.tweetTypes"
						:key="index"
						:value="type.value"
						>{{ type.name }}
					</option>
				</select>
			</div>
			<br />
			<input
				v-if="maxTwitContentLength > 180"
				type="submit"
				value="submit"
				disabled
			/>
			<input v-else type="submit" value="submit" />
		</form>
	</div>
</template>

<script>
import { reactive, computed } from "vue";

export default {
	name: "AddTwitForm",
	
	setup(props, ctx) {
		const state = reactive({
			tweetTypes: [
				{ name: "Draft Twit", value: "draft" },
				{ name: "Instant Twit", value: "instant" },
			],
			twitType: "instant",
			twitContent: "",
		});

		const maxTwitContentLength = computed(() => {
			return state.twitContent.length;
		})

		const onSubmitHandler = () => {
			if (state.twitType === "draft") {
				return;
			}
			ctx.emit("addtwit", { type: state.twitType, content: state.twitContent });
			state.twitContent = "";
		}
		
		return {
			state, onSubmitHandler, maxTwitContentLength
		}
	},
};
</script>

<style lang="scss" scoped>
.form-field {
	margin-top: 1rem;

	.--exceeded {
		color: red;
		border-color: red;
		outline-color: red;
	}
}
</style>
