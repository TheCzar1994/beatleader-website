<script>
	import {createEventDispatcher} from 'svelte';

	import Button from './Button.svelte';

	const dispatch = createEventDispatcher();

	export let values;
	export let multi = false;
	export let value = values && values.length ? values[0] : null;
	export let loadingValue = null;
	let className = null;
	export {className as class};

	async function onChange(newValue) {
		dispatch('change', newValue);
	}
</script>

{#if values}
	<div class={`switch-types ${className ?? ''}`}>
		{#each values as currentValue}
			{#if !currentValue?.component}
				<Button
					icon={currentValue.icon}
					iconFa={currentValue.iconFa}
					loading={multi ? Array.isArray(loadingValue) && loadingValue.includes(currentValue) : loadingValue === currentValue}
					label={currentValue.label}
					title={currentValue.title}
					type={(multi && Array.isArray(value) & value.includes(currentValue)) || (!multi && currentValue === value)
						? 'primary'
						: 'default'}
					color={currentValue?.textColor ?? (currentValue.color ? 'white' : null)}
					bgColor={currentValue?.color ?? null}
					notSelected={!((multi && Array.isArray(value) & value.includes(currentValue)) || (!multi && currentValue === value))}
					url={currentValue.url}
					disabled={currentValue.disabled}
					cls={currentValue?.cls ?? ''}
					on:click={() => onChange(currentValue)} />
			{:else}
				<svelte:component
					this={currentValue.component}
					{...currentValue?.componentProps ?? null}
					on:change={e => {
						onChange({...currentValue, componentValue: e?.detail});
					}} />
			{/if}
		{/each}
	</div>
{/if}

<style>
	.switch-types {
		display: flex;
		justify-content: start;
		align-items: center;
		font-size: 0.75rem;
		text-align: center;
	}

	:global(.multi-select .button) {
		padding: calc(0.45em - 1px) 2em calc(0.45em - 1px) 1em !important;
		opacity: 0.35 !important;
	}
	:global(.multi-select.selected .button) {
		opacity: 1 !important;
		color: #dbdbdb !important;
		background-color: #3273db !important;
	}
	:global(.multi-select .button .icon) {
		top: 0.6em !important;
		margin-right: 0.125em !important;
	}
</style>
