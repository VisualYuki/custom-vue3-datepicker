<template lang="pug">
div.mb-4.position-relative(@click.stop)
	.input-wrap.position-relative
		input.form-control.cursor-pointer.w-auto.mb-2.input-wrap__input( type="text" @click="showDropdown" :placeholder="placeholder" :disabled="disabled" :value="displayValue ? displayValue : 'date are not selected'" )
		.input-wrap__clear.position-absolute.top-50.cursor-pointer(v-if="clearable" @click="clearInput" style="right: 10px; transform: translateY(-40%)" )
			el-icon
				close-bold
	.picker(:class="{'picker_dropdown': !inlineView}")
		day-picker( v-if="startView === 'day'" :visable="isVisableDropdown || inlineView" @dayIsSelected="updateSelectedDay" :selectedDay="selectedDay" )
		time-picker( v-if="startView === 'time'" :visable="isVisableDropdown || inlineView" @timeIsSelected="updateSelectedTime" )
</template>

<script lang="ts">
	import {defineComponent} from "vue";
	import type {PropType} from "vue";
	import DayPicker from "./DayPicker.vue";
	import TimePicker from "./TimePicker.vue";
	import type {Item} from "./PopupPicker";
	//import {startOfToday, getYear, getMonth, addMonths, subMonths} from "date-fns";

	//const today = startOfToday();

	export default defineComponent({
		components: {DayPicker, TimePicker},
		emits: ["update:modelValue"],
		props: {
			startView: {
				type: String as PropType<"time" | "day" | "month" | "year">,
				default: "day",
			},
			placeholder: {
				type: String,
				default: "",
			},
			disabled: {
				type: Boolean,
				default: false,
			},
			modelValue: {
				type: [Date, String],
				default: undefined,
			},
			inlineView: {
				type: Boolean,
				default: false,
			},
			clearable: {
				type: Boolean,
				default: true,
			},
		},
		data() {
			return {
				//selectedYear: getYear(today),
				//selectedMonth: getMonth(today),
				//selectedDay: undefined,
				selectedDay: undefined as Date | undefined,
				selectedTime: undefined as string | undefined,
				displayValue: undefined as string | undefined | Date,
				isVisableDropdown: false,
				//displayDate: today,
			};
		},
		mounted() {
			document.addEventListener("click", () => {
				this.hideDropdown();
			});
		},
		methods: {
			updateSelectedDay(dayInfo: Item) {
				this.displayValue = dayInfo.date;
				this.selectedDay = dayInfo.date;
				this.$emit("update:modelValue", dayInfo.date);
				this.hideDropdown();
			},
			updateSelectedTime(time: string) {
				this.displayValue = time;
				this.selectedTime = time;
				this.$emit("update:modelValue", time);
				this.hideDropdown();
			},
			showDropdown() {
				this.isVisableDropdown = true;
			},
			hideDropdown() {
				this.isVisableDropdown = false;
			},
			clearInput() {
				this.displayValue = undefined;
				this.selectedDay = undefined;
			},
		},
	});
</script>

<style lang="scss" scoped>
	.input-wrap {
		width: max-content;
		cursor: pointer;

		&__input {
			padding-right: 25px;
		}
	}

	.picker {
		z-index: 10;

		&_dropdown {
			position: absolute;
		}
	}
</style>
