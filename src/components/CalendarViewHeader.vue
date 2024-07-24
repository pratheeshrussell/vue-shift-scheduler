<template>
	<div class="cv-header">
		<div class="cv-header-nav">
			<div class="viewSwitcher">
				<select @input="setView($event.target.value)">
					<option v-for="view in viewModeOptions" :key="`view-mode-${view.value}`" :value="view.value"
						:selected="view.value == currentViewMode">
						{{ view.label }}
					</option>
				</select>
			</div>
			<div class="resourceSwitcher" v-if="currentViewMode !== 'month'">
				<select @input="setResource($event.target.value)">
					<option v-for="res in resourceTypeOptions" :key="`res-view-${res.value}`" :value="res.value"
						:selected="res.value == currentResourceType">
						{{ res.label }}
					</option>
				</select>
			</div>
			<button :disabled="!headerProps.previousYear" class="previousYear" aria-label="Previous Year"
				@click.prevent="onInput(headerProps.previousYear)">
				{{ previousYearLabel }}
			</button>
			<button :disabled="!headerProps.previousPeriod" class="previousPeriod" aria-label="Previous Period"
				@click.prevent="onInput(headerProps.previousPeriod)" v-html="previousPeriodLabel" />
			<button class="currentPeriod" aria-label="Current Period"
				@click.prevent="onInput(headerProps.currentPeriod)">
				{{ headerProps.currentPeriodLabel }}
			</button>
			<button :disabled="!headerProps.nextPeriod" class="nextPeriod" aria-label="Next Period"
				@click.prevent="onInput(headerProps.nextPeriod)">
				{{ nextPeriodLabel }}
			</button>
			<button :disabled="!headerProps.nextYear" class="nextYear" aria-label="Next Year"
				@click.prevent="onInput(headerProps.nextYear)">
				{{ nextYearLabel }}
			</button>
		</div>

		<div class="periodLabel">
			<slot name="label">{{ headerProps.periodLabel }}</slot>
		</div>
	</div>
</template>
<script>
export default {
	name: "CalendarViewHeader",
	props: {
		headerProps: {
			type: Object,
			required: true,
		},
		previousYearLabel: { type: String, default: "<<" },
		previousPeriodLabel: { type: String, default: "<" },
		nextPeriodLabel: { type: String, default: ">" },
		nextYearLabel: { type: String, default: ">>" },
		defaultView: { type: String, default: "month" },
		defaultResource: { type: String, default: "user" },
	},
	data() {
		return {
			currentViewMode: "month",
			currentResourceType: "user",
			viewModeOptions: [
				{ label: "Day View", value: "day" },
				{ label: "Week View", value: "week" },
				{ label: "Month View", value: "month" },
			],
			resourceTypeOptions: [
				{ label: "User", value: "user" },
				{ label: "Job", value: "job" },
			],
		}
	},
	methods: {
		onInput(d) {
			this.$emit("input", d)
		},
		setView(value) {
			this.currentViewMode = value;
			this.$emit("view-change", value);
		},
		setResource(value) {
			this.currentResourceType = value;
			this.$emit("resource-change", value);
		},
	},
	created() {
		this.currentViewMode = this.defaultView;
		this.currentResourceType = this.defaultResource;
	},
}
</script>
<style lang="scss">
.cv-header {
	display: flex;
	flex: 0 1 auto;
	flex-flow: row nowrap;
	align-items: center;
	min-height: 2.5em;
	border-width: 1px 1px 0 1px;
}

.cv-header .periodLabel {
	display: flex;
	flex: 1 1 auto;
	flex-flow: row nowrap;
	min-height: 1.5em;
	line-height: 1;
	font-size: 1.5em;
}

.cv-header,
.cv-header button {
	border-style: solid;
	border-color: #ddd;
}

.cv-header-nav,
.cv-header .periodLabel {
	margin: 0.1em 0.6em;
}

.cv-header-nav button,
.cv-header .periodLabel {
	padding: 0.4em 0.6em;
}

.cv-header button {
	box-sizing: border-box;
	line-height: 1em;
	font-size: 1em;
	border-width: 1px;
}
.resourceSwitcher,
.viewSwitcher {
	margin-right: 5px;
	margin-left: 5px;
	select {
		/* Add some basic styling */
		font-size: 16px;
		font-family: Arial, sans-serif;
		padding: 10px;
		border: 1px solid #ccc;
		border-radius: 5px;
		width: 100%;

		/* Add some visual interest */
		background-color: #f9f9f9;
		color: #333;
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);

		/* Style the dropdown arrow */
		appearance: none;
		background-image: url('data:image/svg+xml;charset=UTF-8,<svg xmlns="(link unavailable)" viewBox="0 0 24 24"><path fill="none" stroke="#333" stroke-width="2" d="M7 10l5 5 5-5"/></svg>');
		background-position: right 10px center;
		background-repeat: no-repeat;
		padding-right: 20px;

		/* Style the options */
		option {
			padding: 10px;
		}
	}



	/* Style the focused state */
	select:focus {
		outline: none;
		border-color: #aaa;
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
	}

	/* Style the disabled state */
	select:disabled {
		background-color: #eee;
		color: #999;
	}
}
</style>
