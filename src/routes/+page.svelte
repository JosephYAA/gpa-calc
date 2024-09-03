<script lang="ts">
	import { Slider } from '$lib/components/ui/slider';
	import { Label } from '$lib/components/ui/label';
	export let noCourses = [3];
</script>

<div class="mt-20 flex flex-col items-center justify-center space-y-2">
	<Label for="courses" class="">Number of Courses: {noCourses}</Label>
	<Slider
		id="courses"
		value={noCourses}
		onValueChange={(v) => (noCourses = v)}
		max={10}
		step={1}
		class="max-w-[40%]"
	/>
</div>
<!-- eslint-disable-next-line-->
{#each { length: noCourses[0] } as _, i}
	<li>Course {i}</li>
{/each}
