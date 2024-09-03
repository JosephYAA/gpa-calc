<script lang="ts">
	import { Slider } from '$lib/components/ui/slider';
	import { Label } from '$lib/components/ui/label';
	import { Input } from '$lib/components/ui/input/index.js';

	export let noCourses = [0];

	interface Course {
		id: number;
		name: string;
		credits: number;
		grade: string;
	}

	export let courses = new Array<Course>();

	function changeNoCourses(value: number) {
		noCourses[0] = value;
		for (let i = courses.length; i < noCourses[0]; i++) {
			courses[i] = {
				id: i,
				name: 'Course ' + (i + 1),
				credits: 10,
				grade: 'A-'
			};
		}
		courses.length = noCourses[0];
	}
</script>

<div class="mt-20 flex flex-col items-center justify-center space-y-2">
	<Label for="courses" class="">Number of Courses: {noCourses}</Label>
	<Slider
		id="courses"
		value={noCourses}
		onValueChange={(v) => changeNoCourses(v[0])}
		max={10}
		step={1}
		class="max-w-[40%]"
	/>
</div>
<!-- eslint-disable-next-line-->
<form>
	{#each courses as course}
		<Label for="course-name">Course Name</Label>
		<Input type="text" id="course-name" bind:value={course.name} />
	{/each}
</form>
