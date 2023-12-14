<script lang="ts">
	import RadioGroup from '$lib/RadioGroup.svelte';
</script>

<div class="p-4">
	<h1 class="font-bold text-3xl">Notes:</h1>
	<p class="my-4">This is a reproduction example for <a href="https://github.com/melt-ui/melt-ui/issues/820">this bug</a> in Melt-ui.</p>
	<ul class="list-inside list-disc mb-6">
		<li>Options B and D are disabled.</li>
		<li>A is pre-selected as the initial value.</li>
		<li>You can use your mouse to click option C.</li>
		<li>It is not possible to navigate to option C using only the keyboard (tab, arrow keys).</li>
		<li>The "loop" option is enabled, so with A selected you can hit the "up" arrow key to loop around
			to "E", and then from "E" hit "down" to loop back around to "A".</li>
		<li>From "A" you <em>should be able to</em> (but can't, because of this bug) hit the "down" arrow
			key which would take you to option "C" since "B" is disabled. (And again would take you to
			"E" since "D" is disabled.)</li>
	</ul>
	<RadioGroup initialValue={'a'} options={[
		{ label: 'A', value: 'a' },
		{ label: 'B', value: 'b', disabled: true },
		{ label: 'C', value: 'c' },
		{ label: 'D', value: 'd', disabled: true },
		{ label: 'E', value: 'e' },
	]} />
</div>
