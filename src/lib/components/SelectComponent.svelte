<script lang="ts">
	import { Select } from 'bits-ui';
	import Icon from '@iconify/svelte';

	interface Props {
		items: { value: string; label: string; color: string }[];
		selectedValue: string | string[] | undefined;
		type: 'single' | 'multiple';
		selectedLabel: string;
	}

	let { items = [], selectedValue = $bindable(), type = 'single', selectedLabel = '' }: Props = $props();
</script>

<div>
	<Select.Root {type} bind:value={selectedValue as never} {items}>
		<Select.Trigger
			class="flex w-72 cursor-pointer items-center justify-between rounded-lg border border-gray-300 p-3 transition-all duration-300 ease-in-out select-none hover:border-gray-500"
			aria-label="Select a theme"
		>
			<Icon icon="mdi:palette" />

			<span class="font-medium text-gray-700">
				{selectedLabel}
			</span>
			<Icon icon="mdi:chevron-down" class="ml-auto text-gray-500" />
		</Select.Trigger>

		<Select.Portal>
			<Select.Content
				class="z-50 mt-2 max-h-96 min-w-72 transform overflow-y-auto rounded-xl border border-gray-200 bg-white shadow-lg transition-all duration-300 ease-in-out data-[state=closed]:scale-95 data-[state=open]:scale-100"
			>
				<Select.Viewport class="p-1">
					{#each items as theme, i (i + theme.value)}
						<Select.Item
							class="flex h-12 w-full cursor-pointer items-center rounded-lg px-4 py-2 text-sm text-gray-700 capitalize outline-hidden select-none hover:bg-gray-100"
							value={theme.value}
							label={theme.label}
						>
							{#snippet children({ selected })}
								{theme.label}
								{#if selected}
									<div class="ml-auto">
										<Icon icon="mdi:check" class="text-green-500" />
									</div>
								{/if}
							{/snippet}
						</Select.Item>
					{/each}
				</Select.Viewport>
			</Select.Content>
		</Select.Portal>
	</Select.Root>
</div>
