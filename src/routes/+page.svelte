<script lang="ts">
	import { Slider } from '$lib/components/ui/slider';
	import { Label } from '$lib/components/ui/label';
	import { Input } from '$lib/components/ui/input/index.js';
	import { Button } from '$lib/components/ui/button';
	import * as Alert from '$lib/components/ui/alert';
	import * as Select from '$lib/components/ui/select/index.js';

	export let noCourses = [0];

	interface Course {
		id: number;
		name: string;
		credits: number;
		grade: { label: string; value: number };
	}

	let grades = [
		{ label: 'A', value: 4.0 },
		{ label: 'A-', value: 3.75 },
		{ label: 'B+', value: 3.5 },
		{ label: 'B', value: 3.0 },
		{ label: 'B-', value: 2.75 },
		{ label: 'C+', value: 2.5 },
		{ label: 'C', value: 2.0 },
		{ label: 'C-', value: 1.75 },
		{ label: 'D+', value: 1.3 },
		{ label: 'D', value: 1.0 },
		{ label: 'F', value: 0.0 }
	];

	export let courses = new Array<Course>();

	function changeNoCourses(value: number) {
		noCourses[0] = value;
		for (let i = courses.length; i < noCourses[0]; i++) {
			courses[i] = {
				id: i,
				name: 'Course ' + (i + 1),
				credits: 3,
				grade: { label: 'A', value: 4.0 }
			};
		}
		courses.length = noCourses[0];
	}

	let GPA = 0;
	let visible = false;

	function calculateGPA() {
		let noCredits: number = 0;
		let gradePoints: number = 0;
		for (let i = 0; i < courses.length; i++) {
			noCredits += Number(courses[i].credits);
			gradePoints += courses[i].grade.value * Number(courses[i].credits);
		}
		GPA = gradePoints / noCredits;
		visible = true;
	}
</script>

<div class="mb-10 mt-20 flex flex-col items-center justify-center space-y-2">
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
{#if visible}
	<Alert.Root class="m-auto mb-5 max-w-[20%]">
		<Alert.Title>Congratulations!</Alert.Title>
		<Alert.Description>Your GPA is: {GPA}</Alert.Description>
	</Alert.Root>
{/if}
<form>
	{#each courses as course}
		<div class="mb-2 flex items-center justify-center space-x-3 p-2">
			<Label for="course-{course.id}-name">Course Name</Label>
			<Input
				type="text"
				class="max-w-[20%]"
				id="course-{course.id}-name"
				bind:value={course.name}
				on:change={() => (visible = false)}
			/>
			<Label for="course-{course.id}-credits">Course Credits</Label>
			<Input
				type="number"
				class="max-w-20"
				id="course-{course.id}-credits"
				min="1"
				max="4"
				on:change={() => (visible = false)}
				bind:value={course.credits}
			/>
			<Select.Root
				items={grades}
				onSelectedChange={() => (visible = false)}
				bind:selected={course.grade}
			>
				<Select.Trigger class="max-w-28">
					<Select.Value placeholder="Select your grade" />
				</Select.Trigger>
				<Select.Content>
					<Select.Group>
						<Select.Label>Grades</Select.Label>
						{#each grades as grade}
							<Select.Item value={grade.value} label={grade.label}>{grade.label}</Select.Item>
						{/each}
					</Select.Group>
				</Select.Content>
				<Select.Input name="course-{course.id}-grade" />
			</Select.Root>
		</div>
	{/each}
	<div class="flex justify-center">
		<Button on:click={calculateGPA}>Calculate GPA</Button>
	</div>
</form>
