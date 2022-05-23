<script lang="ts">
	import Paper, { Title, Subtitle, Content } from '@smui/paper';
	import { Input } from '@smui/textfield';
	import Fab from '@smui/fab';
	import { Icon as IconCommon } from '@smui/common';
	import Card from '@smui/card';
	import Button, { Label } from '@smui/button';

	import LayoutGrid, { Cell } from '@smui/layout-grid';
	import Hidden from './hidden.svelte';

	let value = '';
	let parsedData;
	let shown;
	let show;

	async function doSearch() {
		console.log('1');
		const response = await fetch('http://localhost:8080/url/parse-url?url=' + value);
		const data = await response.json();
		console.log('2');
		parsedData = JSON.stringify(data, null, 4);
	}

	async function getZip() {
		console.log('3');
		const response = await fetch('http://localhost:8080/url/zip?url=' + value);
		let blob = await response.blob();
		var url = window.URL || window.webkitURL;
		let link = url.createObjectURL(blob);

		// generate anchor tag, click it for download and then remove it again
		let a = document.createElement('a');
		a.setAttribute('download', `image.zip`);
		a.setAttribute('href', link);
		document.body.appendChild(a);
		a.click();
		document.body.removeChild(a);
		console.log('4');
	}

	async function handleKeyDown(event: CustomEvent | KeyboardEvent) {
		event = event as KeyboardEvent;
		if (event.key === 'Enter') {
			await doSearch();
		}
	}
</script>

<div class="solo-demo-container solo-container">
	<Paper class="solo-paper" elevation={6}>
		<IconCommon class="material-icons">search</IconCommon>
		<Input bind:value on:keydown={handleKeyDown} placeholder="Search" class="solo-input" />
	</Paper>
	<Fab
		on:click={doSearch}
		on:click={show}
		disabled={value === ''}
		color="primary"
		mini
		class="solo-fab"
	>
		<IconCommon class="material-icons">arrow_forward</IconCommon>
	</Fab>
</div>

<LayoutGrid>
	<Cell span={12}>
		<div class="demo-cell" />
	</Cell>
</LayoutGrid>

<Hidden bind:shown bind:show>
	<LayoutGrid>
		<Cell span={2}>
			<div class="demo-cell" />
		</Cell>
		<Cell span={8}>
			<div class="card-display">
				<div class="card-container">
					<Card variant="outlined" padded><p>{parsedData}</p></Card>
				</div>
			</div>
		</Cell>
		<Cell span={2}>
			<div class="demo-cell" />
		</Cell>

		<Cell span={2}>
			<div class="demo-cell" />
		</Cell>
		<Cell span={8}>
			<div class="card-display">
				<div class="card-container">
					<Button
						on:click={() => {
							getZip;
						}}
						variant="raised"
					>
						<Label>Get Zip</Label>
					</Button>
				</div>
			</div>
		</Cell>
		<Cell span={2}>
			<div class="demo-cell" />
		</Cell>
	</LayoutGrid>
</Hidden>

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
