<script lang="ts">
	import { Select } from 'bits-ui';
	import { CaretDoubleDown, CaretDoubleUp, CaretUpDown, Check, Palette } from 'phosphor-svelte';
	import { writable, derived } from 'svelte/store'; // writable für Speichern der Auswahl, derived für abgeleitete Werte wie die ausgewählte Farbe und das Label zu berechnen

	const themes = [
		{ value: 'bea-green', label: 'Bea Test Green', color: '#92997e' },
		{ value: 'dark-green', label: 'Dark Green', color: '#006400' },
		{ value: 'svelte-orange', label: 'Svelte Orange', color: '#FF5E00' },
		{ value: 'punk-pink', label: 'Punk Pink', color: '#D4006C' },
	];

	let multipleValue = writable<string[]>([]);

	const selectedMultipleColors = derived(multipleValue, $multipleValue => {
		const selectedThemes = themes.filter(theme => $multipleValue.includes(theme.value));
		return selectedThemes.map(theme => theme.color);
	});

	const gradientBackground = derived(selectedMultipleColors, $selectedMultipleColors => {
		if ($selectedMultipleColors.length === 0) return '';
		if ($selectedMultipleColors.length === 1) return `bg-${$selectedMultipleColors}`;
		return `linear-gradient(to right, ${$selectedMultipleColors.join(', ')})`;
	});

	const selectedMultipleLabels = derived(multipleValue, $multipleValue => {
		return $multipleValue.length
			? themes
					.filter(theme => $multipleValue.includes(theme.value))
					.map(theme => theme.label)
					.join(', ')
			: 'Select min 2 to create a gradient';
	});

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

<div class="w-full, h-full p-10">
	<div>
		<div>
			<Select.Root type="single" onValueChange={v => value.set(v)} items={themes}>
				<Select.Trigger
					class="flex w-72 cursor-pointer items-center justify-between rounded-lg border border-gray-300 p-3 select-none"
					aria-label="Select a theme"
				>
					<Palette class="mr-3 text-gray-500" />
					{$selectedLabel}
					<CaretUpDown class="ml-auto text-gray-500" />
				</Select.Trigger>
				<Select.Portal>
					<Select.Content
						class="z-50 mt-2 max-h-96 min-w-72 transform overflow-y-auto rounded-xl border border-gray-200 bg-white shadow-lg transition-all duration-300 ease-in-out data-[state=closed]:scale-95 data-[state=open]:scale-100"
					>
						<Select.ScrollUpButton class="flex w-full items-center justify-center py-2">
							<CaretDoubleUp />
						</Select.ScrollUpButton>
						<Select.Viewport class="p-1">
							{#each themes as theme, i (i + theme.value)}
								<Select.Item
									class="rounded-button flex h-10 w-full cursor-pointer items-center rounded-lg py-3 pr-1.5 pl-5 text-sm capitalize outline-hidden select-none hover:bg-gray-100"
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
		<div
			class="mt-5 mb-5 flex h-48 w-full items-center justify-center rounded-xl"
			style="background-color: {$selectedColor};"
		>
			<h1>{$selectedLabel}</h1>
		</div>
	</div>

	<div>
		<div>
			<Select.Root type="multiple" onValueChange={v => multipleValue.set(v)} items={themes}>
				<Select.Trigger
					class="flex w-72 cursor-pointer items-center justify-between rounded-lg border border-gray-300 p-3 select-none"
				>
					{$selectedMultipleLabels}
					<CaretUpDown class="ml-auto text-gray-500" />
				</Select.Trigger>
				<Select.Portal>
					<Select.Content
						class="z-50 mt-2 max-h-96 min-w-72 transform overflow-y-auto rounded-xl border border-gray-200 bg-white shadow-lg transition-all duration-300 ease-in-out data-[state=closed]:scale-95 data-[state=open]:scale-100"
					>
						<Select.ScrollUpButton class="flex w-full items-center justify-center py-2">
							<CaretDoubleUp />
						</Select.ScrollUpButton>
						<Select.Viewport class="p-1">
							{#each themes as theme, i (i + theme.value)}
								<Select.Item
									class="rounded-button flex h-10 w-full items-center rounded-lg py-3 pr-1.5 pl-5 text-sm capitalize outline-hidden select-none hover:bg-gray-100"
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
		<div
			class="mt-5 mb-5 flex h-48 w-full items-center justify-center rounded-xl"
			style="background: {$gradientBackground};"
		>
			<h1>{$selectedMultipleLabels}</h1>
		</div>
	</div>
</div>
