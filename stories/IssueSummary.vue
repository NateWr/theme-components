<template>
	<a :href="url" class="issue-summary" :class="image ? 'issue-summary--has-cover' : ''">
		<div class="issue-summary__wrapper">
			<h3 v-if="volume" class="issue-summary__volume">Vol {{ volume }}, No {{ number }}</h3>
			<div v-if="title" class="issue-summary__title">
				{{ title }}
			</div>
			<div v-if="date" class="issue-summary__date">
				{{ date }}
			</div>
		</div>
		<img
			v-if="image"
			:src="image"
			class="issue-summary__cover-image"
			alt="The alt text entered by the user."
		/>
	</a>
</template>

<script>
export default {
	props: {
		url: String,
		volume: Number,
		number: Number,
		title: String,
		date: String,
		image: String,
		year: Number
	}
};
</script>

<style lang="css">
.issue-summary {
	display: flex;
	margin: 0 0 1rem;
	justify-content: space-between;
	text-decoration: none;
	color: var(--text-color);
}

.issue-summary__wrapper {
	padding-right: 1rem;
}

.issue-summary__title {
	font-size: 0.9rem;
}

h3 {
	margin: 0;
	padding: 0.4rem 0;
	font-size: 1rem;
}

.issue-summary__date {
	margin-top: 0.25rem;
	font-size: 0.8rem;
}

.issue-summary__cover-image {
	display: block;
	max-width: 6rem;
	height: auto;
}

@media (min-width: 500px) {

	.issue-summary__cover-image {
		max-width: 8rem;
	}
}
</style>
