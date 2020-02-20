<template>
	<div class="issue-archives" :class="'issue-archives--' + view">
		<div class="issue-archives__header">
			<div class="issue-archives__header-title">
				<h1>All Issues</h1>
			</div>
			<div class="issue-archives__header-search">
				<form class="issue-archives__header-form">
					<input
						class="issue-archives__header-bar"
						type="text"
						placeholder="Search..."
						required
					/>
					<input
						class="issue-archives__header-button"
						type="button"
						value="Search"
					/>
				</form>
			</div>
		</div>
		<template v-if="view === 'issues'">
			<div class="issue-archives__tab-wrapper">
				<div class="issue-archives__tabs">
					<ul class="issue-archives__tabs-list" role="tablist">
						<li
							v-for="tab in archives"
							:key="tab.year + tab.volume"
							class="issue-archives__tabs-list-item"
							role="presentation"
						>
							<button
								:id="'issue-archive__button-' + tab.year"
								:ref="'button' + tab.year"
								class="issue-archives__button issue-archives__group-name"
								:aria-selected="activeTab.year === tab.year"
								:tabindex="activeTab.year !== tab.year ? '-1' : '0'"
								role="tab"
								type="button"
								@click="activeTab = tab"
								@keyup.right="right()"
								@keyup.left="left()"
							>
								<span class="issue-archives__group-title">
									{{ tab.year }}
								</span>
								<span class="issue-archives__group-separator">—</span>
								<span class="issue-archives__group-subtitle">
									Volume {{ tab.volume }}
								</span>
							</button>
						</li>
					</ul>
				</div>
				<div class="issue-archives__tab-panels">
					<section
						v-for="tab in archives"
						:key="tab.year + tab.volume"
						:aria-labelledby="'issue-archives__button' + tab.year"
						:hidden="activeTab.year !== tab.year"
						tabindex="-1"
						role="tabpanel"
					>
						<h2 class="-screen-reader">{{ year }}</h2>
						<IssueSummary
							v-for="issueSummary in tab.issues"
							:key="issueSummary.volume + issueSummary.year"
							v-bind="issueSummary"
						/>
					</section>
				</div>
			</div>
		</template>
		<template v-else-if="view === 'covers'">
			<div class="issue-archives--covers__volumes">
				<section
					v-for="tab in archives"
					:key="tab.volume + tab.year"
					class="issue-archives--covers__volume"
				>
					<div class="issue-archives__covers-group">
						<h2 class="issue-archives__group-name">
							<span class="issue-archives__group-title">
								{{ tab.year }}
							</span>
							<span class="issue-archives__group-separator">—</span>
							<span class="issue-archives__group-subtitle">
								Volume {{ tab.volume }}
							</span>
						</h2>
					</div>
					<div class="issue-archives--covers__issues">
						<IssueSummary
							v-for="issueSummary in tab.issues"
							:key="issueSummary.volume + issueSummary.year"
							v-bind="issueSummary"
						/>
					</div>
				</section>
			</div>
		</template>
	</div>
</template>

<script>
import IssueSummary from './IssueSummary.vue';

export default {
	components: {IssueSummary},
	props: {
		archives: Object,
		view: String
	},
	data() {
		return {
			activeTab: 0
		};
	},
	watch: {
		activeTab(newVal) {
			if (this.$refs['button' + newVal.year]) {
				this.$refs['button' + newVal.year][0].focus();
			}
		}
	},
	mounted() {
		this.activeTab = this.archives[0];
	},
	methods: {
		right() {
			const i =
				this.archives.findIndex(tab => this.activeTab.year === tab.year) + 1;
			if (i > this.archives.length - 1) {
				this.activeTab = this.archives[0];
			} else {
				this.activeTab = this.archives[i];
			}
		},
		left() {
			const i =
				this.archives.findIndex(tab => this.activeTab.year === tab.year) - 1;
			if (i < 0) {
				this.activeTab = this.archives[this.archives.length - 1];
			} else {
				this.activeTab = this.archives[i];
			}
		}
	}
};
</script>

<style lang="css">
h1 {
	font-size: 1.3rem;
}

.issue-archives__header {
	padding: 0;
	margin: 0;
}

.issue-archives__header-title,
.issue-archives__header-search {
	padding: 0;
	margin: 0;
}

.issue-archives__header-form {
	display: flex;
}

.search {
	padding: 0;
	margin: 0;
	border: 1px solid;
}

.issue-archives__header-button {
	position: relative;
	padding: 0.2rem;
	left: -0.2rem;
	border: 2px solid;
}

/* Tabs */
.issue-archives__tabs {
	border-bottom: 1px solid;
}

.issue-archives__tabs-list {
	display: flex;
	flex-wrap: nowrap;
	overflow: hidden;
	margin: 0;
	padding: 0;
}

.issue-archives__tabs-list-item {
	list-style-type: none;
}

.issue-archives__button {
	background: none;
	color: inherit;
	border: none;
	cursor: pointer;
	padding: 0.75rem 1.75rem;
	font-weight: bold;
}

.issue-archives__button[aria-selected],
.issue-archives__button:hover,
.issue-archives__button:focus {
	border-bottom: 2px solid;
	outline: none;
}

.issue-archives--issues .issue-archives__group-separator,
.issue-archives--issues .issue-archives__group-subtitle {
	display: none;
}

.issue-archives__tab-panels {
	padding-top: 1rem;
}

/* Covers */
.issue-archives--covers__volumes {
	margin-top: 1rem;
}

.issue-archives--covers .issue-archives__group-name {
	font-size: 1rem;
	margin: 0 0 0.25rem;
}

.issue-archives--covers .issue-summary {
	display: inline-block;
	width: 23%;
	vertical-align: top;
}

.issue-archives--covers .issue-summary {
	margin-right: 2%;
}

.issue-archives--covers .issue-summary--has-cover .issue-summary__wrapper,
.issue-archives--covers .issue-summary:not(.issue-summary--has-cover) .issue-summary__title,
.issue-archives--covers .issue-summary:not(.issue-summary--has-cover) .issue-summary__date {
	clip: rect(1px, 1px, 1px, 1px);
	position: absolute !important;
	left: -2000px;
}

.issue-archives--covers .issue-summary--has-cover .issue-summary__wrapper:focus,
.issue-archives--covers .issue-summary:not(.issue-summary--has-cover) .issue-summary__title:focus,
.issue-archives--covers .issue-summary:not(.issue-summary--has-cover) .issue-summary__date:focus {
	background-color: #fff;
	border-radius: 3px;
	box-shadow: 0 0 2px 2px rgba(0, 0, 0, 0.6);
	-webkit-box-shadow: 0 0 2px 2px rgba(0, 0, 0, 0.6);
	clip: auto !important;
	color: #000;
	display: block;
	font-size: 1rem;
	height: auto;
	line-height: normal;
	padding: 1rem;
	position: absolute;
	left: 1rem;
	top: 1rem;
	text-decoration: none;
	width: auto;
	z-index: 100000;
}

.issue-archives--covers .issue-summary--has-cover .issue-summary__cover-image {
	max-width: 100%;
}

.issue-archives--covers .issue-summary:not(.issue-summary--has-cover) {
	background: #ddd;
}

.issue-archives--covers .issue-summary:not(.issue-summary--has-cover) .issue-summary__wrapper {
	padding: 0.75rem;
}

@media (min-width: 767px) {
	.issue-archives__header {
		display: flex;
		align-items: center;
		border-bottom: 1px solid;
	}

	.issue-archives__header-search {
		margin-left: auto;
	}

	.issue-archives--issues .issue-archives__group-separator,
	.issue-archives--issues .issue-archives__group-subtitle {
		display: block;
	}

	.issue-archives__group-name {
		position: relative;
		width: 100%;
		text-align: left;
	}

	.issue-archives__group-title {
		position: relative;
		padding-right: 0.5em;
		background: var(--background-color);
		z-index: 2;
	}

	.issue-archives__group-separator {
		display: block;
		position: absolute;
		top: 50%;
		left: 1rem;
		right: 1rem;
		height: 1px;
		overflow: hidden;
		border-top: 1px solid;
		z-index: -1;
		opacity: 0.3;
	}

	.issue-archives__group-subtitle {
		display: block;
		position: relative;
		float: right;
		padding-left: 0.5em;
		background: var(--background-color);
		font-weight: normal;
	}

	/* Tabs */
	.issue-archives__tab-wrapper {
		display: flex;
	}

	.issue-archives__tabs {
		border-bottom: none;
		border-right: 1px solid;
	}

	.issue-archives__tabs-list {
		display: block;
		min-width: 12rem;
		margin-top: 1rem;
		margin-right: 1rem;
	}

	.issue-archives__button {
		border-bottom: none;
		border-left: 2px solid transparent;
		padding-left: 1rem;
		padding-right: 1rem;
	}

	.issue-archives__button[aria-selected],
	.issue-archives__button:hover,
	.issue-archives__button:focus {
		border-bottom: none;
		border-left: 2px solid;
	}

	.issue-archives__tab-panels {
		padding-left: 2rem;
		flex: 1;
	}

	/* Covers */
	.issue-archives--covers__volume {
		display: flex;
		margin-top: 1rem;
	}

	.issue-archives__covers-group {
		margin: 0 1rem 0 0;
		width: 25%;
		font-size: 1rem;
		font-weight: bold;
	}

	.issue-archives--covers__issues {
		width: 100%;
	}

	.issue-archives--covers .issue-summary {
		background: #ddd;
	}

	.issue-archives--covers .issue-summary__wrapper {
		padding: 0.75rem;
	}
}
</style>
