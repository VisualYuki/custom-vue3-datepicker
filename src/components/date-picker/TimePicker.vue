<template lang="pug">
popup-picker( v-if="visable" :leftArrowIsDisabled="true" :rightArrowIsDisabled="true" )
	template( #head ) {{ selectedHours }} : {{ selectedMunits }}
	template( #body )
		el-row
			el-col( :span="12" style="padding: 0 5px" )
				el-scrollbar( height="200px" :native="true" )
					div.d-flex.flex-column.align-items-stretch
						el-button( v-for="hours in getCorrectNumbers(23)" style="margin-left: 0;" :type="hours === selectedHours ? 'primary': ''"  @click="selectHours(hours)" ) {{ hours }}
			el-col( :span="12" style="padding: 0 5px" )
				el-scrollbar( height="200px" :native="true" )
					div.d-flex.flex-column.align-items-stretch
						el-button( v-for="munits in getCorrectNumbers(59)" style="margin-left: 0;" :type="munits === selectedMunits ? 'primary': ''" @click="selectMunits(munits)" ) {{ munits }}
</template>

<script lang="ts">
	import {defineComponent} from "vue";
	import PopupPicker from "./PopupPicker.vue";

	export default defineComponent({
		name: "TimePicker",
		components: {
			PopupPicker,
		},
		props: {
			visable: {
				type: Boolean,
				default: false,
			},
		},
		emits: ["timeIsSelected"],
		data() {
			return {
				selectedHours: undefined as string | undefined,
				selectedMunits: undefined as string | undefined,
			};
		},
		methods: {
			checkValidSelection() {
				if (this.selectedHours && this.selectedMunits) {
					this.$emit("timeIsSelected", this.selectedHours + " : " + this.selectedMunits);
				}
			},
			getCorrectNumbers(max: number): string[] {
				const result: string[] = [];

				for (let i = 0; i <= max; i++) {
					result.push(this.addLeadingZero(i));
				}

				return result;
			},
			addLeadingZero(number: number): string {
				let result: string = number.toString();

				if (number < 10) {
					result = "0" + result;
				}

				return result;
			},
			selectHours(hours: string) {
				this.selectedHours = hours;
				this.checkValidSelection();
			},
			selectMunits(munits: string) {
				this.selectedMunits = munits;
				this.checkValidSelection();
			},
		},
	});
</script>

<style lang="scss" scoped></style>
