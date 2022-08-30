<template lang="pug">
popup-picker(v-if="visable" :bodyItems="monthDays" @dayIsSelected="dayIsSelected" @leftArrowIsClicked="subMonth" @rightArrowIsClicked="addMonth" )
	template( #head ) {{selectedYear}} {{selectedMonth}}
	template( #subheader )
		span.text-center( v-for="day in weekDays" ) {{day}}
</template>

<script lang="ts">
	import {defineComponent} from "vue";
	import {
		startOfToday,
		startOfMonth,
		getDay,
		getDaysInMonth,
		eachDayOfInterval,
		endOfMonth,
		isEqual,
		getYear,
		format,
		addMonths,
		subMonths,
		addDays,
	} from "date-fns";
	import {enUS} from "date-fns/locale";
	import type {Item} from "./PopupPicker";
	import PopupPicker from "./PopupPicker.vue";

	const today = startOfToday();

	export default defineComponent({
		name: "DayPicker",
		props: {
			selectedDay: {
				type: Date,
				default: undefined,
			},
			visable: {
				type: Boolean,
				default: false,
			},
		},
		components: {
			PopupPicker,
		},
		data() {
			return {
				weekDays: ["Mo", "Tu", "We", "Th", "Fr", "Sa", "Su"],
				displayDate: today,
			};
		},

		computed: {
			selectedYear(): number {
				return getYear(this.displayDate);
			},
			selectedMonth() {
				return format(this.displayDate, "LLLL", {locale: enUS});
			},
			monthDays(): Item[] {
				const result: Item[] = [];
				const startMonthDay = startOfMonth(this.displayDate);

				{
					const prevMonth: Date = subMonths(this.displayDate, 1);
					const daysInPrevMonth = getDaysInMonth(prevMonth);
					const weekDay = getDay(startMonthDay);

					for (let i = daysInPrevMonth - weekDay + 1; i <= daysInPrevMonth; i++) {
						result.push({
							display: i,
							selected: false,
							disabled: true,
						});
					}
				}

				{
					const endMonthDay = endOfMonth(this.displayDate);

					eachDayOfInterval({
						start: startMonthDay,
						end: endMonthDay,
					}).forEach((day: Date) => {
						result.push({
							display: day.getDate(),
							date: day,
							selected: this.selectedDay !== undefined && isEqual(day, this.selectedDay),
							disabled: false,
						});
					});
				}

				return result;
			},
		},
		methods: {
			dayIsSelected(dayInfo: Item) {
				this.$emit("dayIsSelected", dayInfo);
			},
			addMonth() {
				this.displayDate = addMonths(this.displayDate, 1);
			},
			subMonth() {
				this.displayDate = subMonths(this.displayDate, 1);
			},
		},
	});
</script>

<style lang="scss" scoped></style>
