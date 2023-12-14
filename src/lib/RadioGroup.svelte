<script lang="ts">
	import { createRadioGroup, melt } from '@melt-ui/svelte';

	export let initialValue: string;
	export let options: string[] | Array<{ value: string; label: string; disabled?: boolean }>;
	export let orientation: 'horizontal' | 'vertical' = 'vertical';
	export let disabled: boolean = false;
	export let required: boolean = false;

	/**
	 * Default `true`. When `true`, arrow key navigation loops around.
	 * When `false`, arrow key navigation stops at the last item.
	 */
	export let loop: boolean = true;

	/**
	 * Use this attribute to set an `aria-label` on the wrapping div; e.g. "View Density"
	 * when selcting comfy/compact view density.
	 */
	export let ariaLabel: string | undefined = undefined;

const {
	elements: { root, item, hiddenInput },
	helpers: { isChecked }
} = createRadioGroup({
	defaultValue: initialValue,
	disabled,
	required,
	loop,
	orientation
});

	const useOptions = options.map((o) => {
		if (typeof o === 'string') {
			return { value: o, label: o, safe: makeSafe(o) };
		}
		return { ...o, safe: makeSafe(o.label) };
	});

	function makeSafe(str: string) {
		//labels and values can start with a number, but ID's can't
		return '_' + str.toLowerCase().replace(/[^a-z0-9]/g, '-');
	}
</script>

<div
	use:melt={$root}
	class="flex flex-col gap-3 data-[orientation=horizontal]:flex-row"
	aria-label={ariaLabel}
>
	{#each useOptions as option}
		{@const buttonClasses = [
			'grid',
			'h-6 w-6',
			'cursor-default',
			'place-items-center',
			'rounded-full',
			option.disabled ? 'bg-neutral-200' : 'bg-white',
			'border-2',
			'border-neutral-300',
			'dark:border-slate-700 dark:bg-slate-700',
			'focus-within:ring-0 focus-within:outline-none focus-within:!border-sky-500'
		].join(' ')}
		<div class="flex items-center gap-3 {option.disabled ? '' : 'hover:opacity-75'}">
			<button
				use:melt={$item(option)}
				class={buttonClasses}
				id={option.safe}
				aria-labelledby="{option.safe}-label"
			>
				{#if $isChecked(option.value)}
					<div class="h-3 w-3 rounded-full bg-sky-500" />
				{/if}
			</button>
			<label
				class="capitalize leading-none select-none {option.disabled ? 'text-neutral-400' : ''}"
				for={option.safe}
				id="{option.safe}-label"
			>
				{option.label}
			</label>
		</div>
	{/each}
	<input name="line-height" use:melt={$hiddenInput} />
</div>
