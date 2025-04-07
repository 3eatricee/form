<script lang="ts">
	import { Select } from 'bits-ui';
	import { CaretDoubleDown, CaretDoubleUp, CaretUpDown, Check, Palette } from 'phosphor-svelte';
	import { writable, derived } from 'svelte/store'; // writable für Speichern der Auswahl, derived für abgeleitete Werte wie die ausgewählte Farbe und das Label zu berechnen

	const themes = [
		{ value: 'light-monochrome', label: 'Light Monochrome', color: '#f5f5f5' },
		{ value: 'dark-green', label: 'Dark Green', color: '#006400' },
		{ value: 'svelte-orange', label: 'Svelte Orange', color: '#FF5E00' },
		{ value: 'punk-pink', label: 'Punk Pink', color: '#D4006C' },
	];

	const value = writable<string>('');

	const selectedColor = derived(value, $value => {
		const selectedTheme = themes.find(theme => theme.value === $value);
		return selectedTheme ? selectedTheme.color : '#ffffff';
	});

	const selectedLabel = derived(value, $value => {
		const selectedTheme = themes.find(theme => theme.value === $value);
		return selectedTheme ? selectedTheme.label : 'Select a theme';
	});
</script>

<div class="flex h-48 w-full items-center justify-center rounded-xl" style="background-color: {$selectedColor};">
	<h1>{$selectedLabel}</h1>
</div>

<div>
	<Select.Root type="single" onValueChange={v => value.set(v)} items={themes}>
		<Select.Trigger
			class="flex w-72 cursor-pointer items-center justify-between rounded-lg border border-gray-300 p-3"
			aria-label="Select a theme"
		>
			<Palette class="mr-3 text-gray-500" />
			{$selectedLabel}
			<CaretUpDown class="ml-auto text-gray-500" />
		</Select.Trigger>
		<Select.Portal>
			<Select.Content
				class="focus-override border-muted bg-background shadow-popover data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 z-50 h-96 max-h-[var(--bits-select-content-available-height)] w-[var(--bits-select-anchor-width)] min-w-[var(--bits-select-anchor-width)] rounded-xl border px-1 py-3 outline-hidden select-none data-[side=bottom]:translate-y-1 data-[side=left]:-translate-x-1 data-[side=right]:translate-x-1 data-[side=top]:-translate-y-1"
			>
				<Select.ScrollUpButton class="flex w-full items-center justify-center py-2">
					<CaretDoubleUp />
				</Select.ScrollUpButton>
				<Select.Viewport class="p-1">
					{#each themes as theme, i (i + theme.value)}
						<Select.Item
							class="rounded-button data-highlighted:bg-muted flex h-10 w-full items-center py-3 pr-1.5 pl-5 text-sm capitalize outline-hidden select-none  data-disabled:opacity-50"
							value={theme.value}
							label={theme.label}
						>
							{#snippet children({ selected })}
								{theme.label}
								{#if selected}
									<div class="ml-auto">
										<Check />
									</div>
								{/if}
							{/snippet}
						</Select.Item>
					{/each}
				</Select.Viewport>
				<Select.ScrollDownButton class="flex w-full items-center justify-center py-2">
					<CaretDoubleDown />
				</Select.ScrollDownButton>
			</Select.Content>
		</Select.Portal>
	</Select.Root>
</div>
