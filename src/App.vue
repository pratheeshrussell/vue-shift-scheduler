<template>
	<div id="app">
		<CalendarView  v-if="schedulerView=='month'"
			:show-date="showDate" 
			:items="items" :enable-date-selection="true"
			:selection-start="selectionStart" :selection-end="selectionEnd" :display-week-numbers="false"
			:item-top="themeOptions.top" :item-content-height="themeOptions.height"
			:item-border-height="themeOptions.border" :class="`theme-` + theme"
			:current-period-label="themeOptions.currentPeriodLabel" class="holiday-us-traditional holiday-us-official"
			@date-selection-start="setSelection" @date-selection="setSelection"
			@date-selection-finish="finishSelection" 
			>
			<calendar-view-header slot="header" slot-scope="{ headerProps }" :header-props="headerProps"
				:previous-year-label="themeOptions.previousYearLabel"
				:previous-period-label="themeOptions.previousPeriodLabel"
				:next-period-label="themeOptions.nextPeriodLabel" :next-year-label="themeOptions.nextYearLabel"
				@input="setShowDate" 
				@view-change="schedulerView=$event" :defaultView="schedulerView" 
				@resource-change="schedulerMode=$event" :defaultResource="schedulerMode"
				/>
		</CalendarView>

		<SchedulerWeekView v-if="schedulerView=='week'"
			:show-date="showDate" 
			:items="items" :enable-date-selection="true"
			:selection-start="selectionStart" :selection-end="selectionEnd" :display-week-numbers="false"
			:item-top="themeOptions.top" :item-content-height="themeOptions.height"
			:item-border-height="themeOptions.border" :class="`theme-` + theme"
			:current-period-label="themeOptions.currentPeriodLabel" class="holiday-us-traditional holiday-us-official"
			@date-selection-start="setSelection" @date-selection="setSelection"
			@date-selection-finish="finishSelection"
			:display-resource="schedulerMode">
			<calendar-view-header slot="header" slot-scope="{ headerProps }" :header-props="headerProps"
				:previous-year-label="themeOptions.previousYearLabel"
				:previous-period-label="themeOptions.previousPeriodLabel"
				:next-period-label="themeOptions.nextPeriodLabel" :next-year-label="themeOptions.nextYearLabel"
				@input="setShowDate"
				@view-change="schedulerView=$event" :defaultView="schedulerView" 
				@resource-change="schedulerMode=$event" :defaultResource="schedulerMode"
				 />
		</SchedulerWeekView>

		<SchedulerDailyView v-if="schedulerView=='day'"
			:show-date="showDate" 
			:items="items" :enable-date-selection="true"
			:selection-start="selectionStart" :selection-end="selectionEnd" :display-week-numbers="false"
			:item-top="themeOptions.top" :item-content-height="themeOptions.height"
			:item-border-height="themeOptions.border" :class="`theme-` + theme"
			:current-period-label="themeOptions.currentPeriodLabel" class="holiday-us-traditional holiday-us-official"
			@date-selection-start="setSelection" @date-selection="setSelection"
			@date-selection-finish="finishSelection"
			:display-resource="schedulerMode">
			<calendar-view-header slot="header" slot-scope="{ headerProps }" :header-props="headerProps"
				:previous-year-label="themeOptions.previousYearLabel"
				:previous-period-label="themeOptions.previousPeriodLabel"
				:next-period-label="themeOptions.nextPeriodLabel" :next-year-label="themeOptions.nextYearLabel"
				@input="setShowDate"
				@view-change="schedulerView=$event" :defaultView="schedulerView" 
				@resource-change="schedulerMode=$event" :defaultResource="schedulerMode"
				 />
		</SchedulerDailyView>
		
	</div>
</template>

<script>
import SchedulerWeekView from "./components/SchedulerWeekView.vue"
import SchedulerDailyView from "./components/SchedulerDailyView.vue"
import CalendarViewHeader from "./components/CalendarViewHeader.vue"
import CalendarView from "./components/CalendarView.vue"
export default {
	name: "CalendarDemoApp",
	components: {
		SchedulerWeekView,
		SchedulerDailyView,
		CalendarViewHeader,
		CalendarView,
	},
	data: function () {
		return {
			showDate: new Date(),
			selectionStart: null,
			selectionEnd: null,
			schedulerView: "month",// day/week/month
			schedulerMode: "user",// user/job
			theme: "gcal",
			// items: Array(5):
			// 	.fill()
			// 	.map((_, i) => this.getRandomEvent(i)),
			items: [
				{
					"id": 0,
					"title": "Job 1",
					"startDate": Date.parse("2024-07-16T12:00:00"),
					"endDate": Date.parse("2024-07-20T12:34:56"),
					"users": [
						{ id: 0, name: "User 1" },
						{ id: 1, name: "User 2" },
					],
				},
				{
					"id": 1,
					"title": "Job 2",
					"startDate": Date.parse("2024-07-20T18:30:56"),
					"endDate": Date.parse("2024-07-23T12:34:56"),
					"users": [
						{ id: 0, name: "User 1" },
						{ id: 2, name: "User 3" },
					],
				},
				{
					"id": 2,
					"title": "Job 3",
					"startDate": Date.parse("2024-07-23T00:34:56"),
					"endDate": Date.parse("2024-07-29T12:34:56"),
					"users": [
						{ id: 3, name: "User 4" },
						{ id: 4, name: "User 5" },
					],
				},
				{
					"id": 3,
					"title": "Job 4",
					"startDate": Date.parse("2024-07-25T12:34:56"),
					"endDate": Date.parse("2024-07-30T12:34:56"),
					"users": [
						{ id: 0, name: "User 1" },
						{ id: 1, name: "User 2" },
					],
				}, {
					"id": 4,
					"title": "Job 5",
					"startDate": Date.parse("2024-07-25T12:34:56"),
					"endDate": Date.parse("2024-07-26T12:34:56"),
					"users": [
						{ id: 2, name: "User 3" },
						{ id: 3, name: "User 4" },
					],
				}],

			unavailableDays:[
				{
					userid: 1,
					days: [Date("2024-07-25T12:34:56"), Date("2024-07-26T12:34:56")],
				}
			],
		}
	},
	computed: {
		themeOptions() {
			return this.theme == "gcal"
				? {
					top: "2.6em",
					height: "2.1em",
					border: "0px",
					previousYearLabel: "\uE5CB\uE5CB",
					previousPeriodLabel: "\uE5CB",
					nextPeriodLabel: "\uE5CC",
					nextYearLabel: "\uE5CC\uE5CC",
					currentPeriodLabel: "Today",
				}
				: {
					top: "1.4em",
					height: "1.4em",
					border: "2px",
					previousYearLabel: "<<",
					previousPeriodLabel: "<",
					nextPeriodLabel: ">",
					nextYearLabel: ">>",
					currentPeriodLabel: "",
				}
		},
	},
	methods: {
		setShowDate(d) {
			this.showDate = d
		},
		setSelection(dateRange) {
			this.selectionEnd = dateRange[1]
			this.selectionStart = dateRange[0]
		},
		finishSelection(dateRange) {
			this.setSelection(dateRange)
		},
		getRandomEvent(index) {
			const startDay = Math.floor(Math.random() * 28 + 1)
			const endDay = Math.floor(Math.random() * 4) + startDay
			var d = new Date()
			var i = {
				id: index,
				title: "Event " + (index + 1),
				startDate: Date.UTC(d.getUTCFullYear(), d.getUTCMonth(), startDay),
				endDate: Date.UTC(d.getUTCFullYear(), d.getUTCMonth(), endDay),
			}
			return i
		},
	},
}
</script>

<style lang="scss">
@import "../static/css/gcal.css";
//@import "../static/css/default.css";
@import "../static/css/holidays-us.css";

div#app {
	font-family: Avenir, Arial, Helvetica, sans-serif;
	display: flex;
	height: 87vh;
	width: 87vw;
	margin-left: 6vw;
}
</style>
