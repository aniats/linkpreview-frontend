<script lang="ts">
	import Paper, { Title, Subtitle, Content } from '@smui/paper';
	import { Input } from '@smui/textfield';
	import Fab from '@smui/fab';
	import { Icon as IconCommon } from '@smui/common';
	import LayoutGrid, { Cell } from '@smui/layout-grid';
	import ImageList, {
		Item,
		ImageAspectContainer,
		Image,
		Supporting,
		Label as LabelImageList
	} from '@smui/image-list';
	let value = '';

	async function doSearch() {
		console.log('1');
		const response = await fetch('http://localhost:8080/url/parse-url?url=' + value);
		const data = await response.json();
		console.log('2');
		alert('Search for ' + JSON.stringify(data));
	}

	async function handleKeyDown(event: CustomEvent | KeyboardEvent) {
		event = event as KeyboardEvent;
		if (event.key === 'Enter') {
			await doSearch();
		}
	}

	function getUnevenImageSize(
		counter: number,
		base: number,
		variance: number,
		preAdd = (num: number) => num
	) {
		const mid = (counter % 2 ? Math.cos : Math.sin)(counter) * variance;
		return base + Math.floor(preAdd(mid));
	}
</script>

<div class="solo-demo-container solo-container">
	<Paper class="solo-paper" elevation={6}>
		<IconCommon class="material-icons">search</IconCommon>
		<Input bind:value on:keydown={handleKeyDown} placeholder="Search" class="solo-input" />
	</Paper>
	<Fab on:click={doSearch} disabled={value === ''} color="primary" mini class="solo-fab">
		<IconCommon class="material-icons">arrow_forward</IconCommon>
	</Fab>
</div>

<pre class="status">Value: {value}</pre>

<ImageList class="my-image-list-masonry" masonry>
	{#each Array(15) as _unused, i}
		<Item>
			<Image
				src="https://place-hold.it/190x{getUnevenImageSize(
					i,
					107,
					200,
					Math.abs
				)}?text=190x{getUnevenImageSize(i, 107, 200, Math.abs)}&fontsize=23"
				alt="Image {i + 1}"
			/>
			<Supporting>
				<LabelImageList>Image {i + 1}</LabelImageList>
			</Supporting>
		</Item>
	{/each}
</ImageList>

<style>
	.solo-demo-container {
		padding: 36px 18px;
		background-color: var(--mdc-theme-background, #f8f8f8);
		border: 1px solid var(--mdc-theme-text-hint-on-background, rgba(0, 0, 0, 0.1));
		height: 400px;
	}

	.solo-container {
		display: flex;
		justify-content: center;
		align-items: center;
	}
	* :global(.solo-paper) {
		display: flex;
		align-items: center;
		flex-grow: 1;
		max-width: 600px;
		margin: 0 12px;
		padding: 0 12px;
		height: 48px;
	}
	* :global(.solo-paper > *) {
		display: inline-block;
		margin: 0 12px;
	}
	* :global(.solo-input) {
		flex-grow: 1;
		color: var(--mdc-theme-on-surface, #000);
	}
	* :global(.solo-input::placeholder) {
		color: var(--mdc-theme-on-surface, #000);
		opacity: 0.6;
	}
	* :global(.solo-fab) {
		flex-shrink: 0;
	}
	.demo-cell {
		height: 60px;
		display: flex;
		justify-content: center;
		align-items: center;
	}
</style>
