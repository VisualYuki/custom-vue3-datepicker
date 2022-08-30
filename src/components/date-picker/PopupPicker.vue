<template lang="pug">
.popup-picker.shadow.p-2
	.popover-header
		.header
			el-button( type="info" plain @click="$emit('leftArrowIsClicked')" :disabled="leftArrowIsDisabled" )
				el-icon( :disabled="leftArrowIsDisabled" )
					arrow-left
			el-button.head.fw-bold.text-black( type="info" plain )
				slot( name="head" )
			el-button( type="info" plain @click="$emit('rightArrowIsClicked')" :disabled="rightArrowIsDisabled" )
				el-icon( :disabled="rightArrowIsDisabled" )
					arrow-right
		.subheader.mt-2
			slot( name="subheader" )
	.popover-body
		slot(name="body")
			div.d-grid(:style="{ 'grid-template-columns': `repeat(${columnCount}, 1fr)` }")
				el-button.text-center.btn.btn-sm.m-1( v-for="(dayInfo, index) in bodyItems" :key="index" :type="(dayInfo.selected) ? 'primary' : ''" :disabled="dayInfo.disabled" style="padding: 0; margin-left: 0" @click="$emit('dayIsSelected', dayInfo)" ) {{ dayInfo.display}}
</template>

<script lang="ts">
	import {defineComponent} from "vue";
	import type {PropType} from "vue";
	import type {Item} from "./PopupPicker";

	export default defineComponent({
		name: "PopupPicker",
		emits: ["leftArrowIsClicked", "rightArrowIsClicked", "dayIsSelected"],
		props: {
			columnCount: {
				type: Number,
				default: 7,
			},
			leftArrowIsDisabled: {
				type: Boolean,
				deafult: false,
			},
			rightArrowIsDisabled: {
				type: Boolean,
				deafult: false,
			},
			rightArrowIsClicked: {
				type: Boolean,
				deafult: false,
			},
			headingClickable: {
				type: Boolean,
				default: false,
			},
			bodyItems: {
				type: Array as PropType<Item[]>,
				default: () => new Array<Item>(),
			},
		},
	});
</script>

<style lang="scss" scoped>
	.popup-picker {
		width: 100%;
		max-width: 300px;
		border-radius: 5px;
		min-width: 300px;
	}

	.popover-body {
		display: grid;
	}

	.header {
		display: flex;
		justify-content: space-between;
		align-items: center;

		.head {
			flex-grow: 1;
		}
	}

	.subheader {
		display: grid;
		grid-template-columns: repeat(7, 1fr);
	}
</style>
