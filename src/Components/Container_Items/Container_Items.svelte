<script>
	import { game_data, game_translations } from '../../stores.js';
	import { get_translated_string, get_icon_path } from '../../utils.js';
	import Info_Item from './../Info_Item/Info_Item.svelte';
	import Container from '../Container/Container.svelte';

	export let heading;
	export let fold_direction;
	export let data_items;
	export let data_translations;
</script>

<div class="items_container">
	<Container {fold_direction}>
		<h2>{heading}</h2>
		<div class="items styled_scrollbar">
			{#if data_items}
				{#each data_items as item}
					<Info_Item
						img_src={get_icon_path(item.id)}
						img_alt={get_translated_string(data_translations, item.id, 'en')}
						tier={item.tier}
						id={item.id}
						name={get_translated_string(data_translations, item.id, 'en')}
					/>
				{/each}
			{/if}
		</div>
	</Container>
</div>

<style>
	h2 {
		text-align: center;
		padding-bottom: 1.5rem;
	}

	.items_container {
		position: relative;
	}

	.items {
		display: grid;
		grid-template-columns: repeat(auto-fit, calc(var(--item-img-size) + var(--item-padding) * 2));
		gap: 1rem;
		max-height: 13rem;
		overflow-y: auto;
	}
</style>
