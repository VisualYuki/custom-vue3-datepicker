<template lang="pug">
.p-5.d-flex
	date-picker( start-view="day" v-model="customModel" :inlineView="true" style="margin-right: 50px;" )
	date-picker( start-view="time" :clearable = "false" :inlineView="true")
</template>

<script lang="ts">
	import datepicker from "./components/date-picker/DatePicker.vue";
	import {defineComponent} from "vue";

	export default defineComponent({
		components: {"date-picker": datepicker},
		data() {
			return {
				customModel: undefined,
			};
		},
	});
</script>

<style scoped lang="scss"></style>
