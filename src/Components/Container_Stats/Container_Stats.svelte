<script>
	import { fly } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';
	import { game_data, game_translations } from '../../stores.js';
	import { get_stat_icon_name, get_translated_string } from '../../utils.js';
	import BtnStatSwitch from '../Btn_Stat_Switch/Btn_Stat_Switch.svelte';
	import Container from '../Container/Container.svelte';
	import InfoStat from '../Info_Stat/Info_Stat.svelte';

	let data_stats_primary;
	let data_stats_secondary;
	let data_translations;
	let current_stat_view = 'primary';

	async function get_game_data() {
		const data = await game_data.get();
		data_stats_primary = data.stats_primary;
		data_stats_secondary = data.stats_secondary;
	}

	async function get_translation_data() {
		const data = await game_translations.get();
		data_translations = data;
	}

	get_game_data();
	get_translation_data();
</script>

<div class="stats_container">
	<Container fold_direction="right">
		<div class="stats">
			<h2>Stats</h2>

			<section class="buttons">
				<BtnStatSwitch
					btn_text="Primary"
					is_active={current_stat_view === 'primary' ? true : false}
					on:click={() => {
						current_stat_view = 'primary';
					}}
				/>
				<BtnStatSwitch
					btn_text="Secondary"
					is_active={current_stat_view === 'secondary' ? true : false}
					on:click={() => {
						current_stat_view = 'secondary';
					}}
				/>
			</section>

			{#if data_stats_primary && current_stat_view === 'primary'}
				<div
					class="info_stats_primary"
					transition:fly|local={{ x: 400, duration: 200, easing: quintOut }}
				>
					{#each Object.keys(data_stats_primary) as stat_primary}
						<InfoStat
							img_src={`static/stat_icons/${get_stat_icon_name(stat_primary)}`}
							stat_text={data_translations[`${stat_primary.toUpperCase()}`].en}
							stat_value={data_stats_primary[stat_primary]}
						/>
					{/each}
				</div>
			{/if}
			{#if data_stats_secondary && current_stat_view === 'secondary'}
				<div
					class="info_stats_secondary"
					transition:fly|local={{ x: 400, duration: 200, easing: quintOut }}
				>
					{#each Object.keys(data_stats_secondary) as stat_secondary}
						<InfoStat
							stat_text={get_translated_string(data_translations, stat_secondary, 'en')}
							stat_value={data_stats_secondary[stat_secondary]}
						/>
					{/each}
				</div>
			{/if}
		</div>
	</Container>
</div>

<style>
	.stats_container {
		grid-column: 3 / 4;
		position: relative;
		z-index: 2;
	}

	.stats {
		display: grid;
		grid-template-columns: 1fr;
		grid-template-rows: max-content max-content 1fr;
		align-items: center;
		width: 22rem;
		height: 38rem;
	}

	h2 {
		text-align: center;
		padding-bottom: 1.5rem;
	}

	.buttons {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 3rem;
		padding-bottom: 2rem;
	}

	.info_stats_primary {
		row-gap: 0.3rem;
	}

	.info_stats_primary,
	.info_stats_secondary {
		width: 100%;
		display: grid;
		grid-column: 1 / 2;
		grid-row: 3 / 4;
	}
</style>
