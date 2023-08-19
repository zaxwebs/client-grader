<script>
	import { Input } from '$components/ui/input'
	import { Label } from '$components/ui/label'
	import { Slider } from '$components/ui/slider'
	import { Switch } from '$components/ui/switch'

	import { Info } from 'lucide-svelte'

	let factors = [
		{ name: 'Communication', weight: 3, rating: 4 },
		{ name: 'Budget', weight: 3, rating: 4 },
		{ name: 'Punctuality', weight: 3, rating: 4 },
		{ name: 'Professionalism', weight: 3, rating: 4 },
		{ name: 'On-Time Payment', weight: 3, rating: 4 },
	]

	$: totalWeight = factors.reduce((sum, factor) => {
		return sum + Number(factor.weight)
	}, 0)

	$: scores = factors.map((factor) => (factor.rating * (factor.weight / totalWeight)).toFixed(2))

	$: totalScore = scores.reduce((sum, score) => {
		return sum + Number(score)
	}, 0)

	$: normalizedScore = ((totalScore / factors.length) * 100).toFixed(0)

	const calculateGrade = (score) => {
		switch (true) {
			case score >= 90:
				return 'A+'
			case score >= 80:
				return 'A'
			case score >= 70:
				return 'B'
			case score >= 60:
				return 'C'
			case score >= 50:
				return 'D'
			default:
				return 'F'
		}
	}

	$: grade = calculateGrade(normalizedScore)

	let weightCustomizer
</script>

<div class="mx-auto max-w-4xl px-4 py-12">
	<h1 class="scroll-m-20 text-4xl font-extrabold tracking-tight lg:text-5xl mb-4">
		Client Score Chronicles
	</h1>
	<p class="mb-8 text-muted-foreground">
		Say goodbye to uncertainty and hello to a data-driven approach that helps you prioritize
		clients and ensure a smoother freelancing journey.
	</p>
	<div class="flex items-center justify-end gap-2 mb-4">
		<Switch id="weight-customizer" bind:rootChecked={weightCustomizer} />
		<Label for="weight-customizer">Weights Customizer</Label>
	</div>
	<table class="h-full w-full table-auto mb-6">
		<thead>
			<tr class="flex border-y border-stroke dark:border-strokedark">
				<th class="w-[40%] py-6 pl-4 pr-4 lg:pl-10 xl:w-1/4">
					<p class="text-left font-medium">Factor</p>
				</th>
				<th class="w-3/5 py-6 px-4">
					<p class="text-left font-medium">Rating</p>
				</th>
				{#if weightCustomizer}
					<th class="w-[35%] py-6 pl-4 pr-4 lg:pr-10 xl:w-[10%]">
						<p class="text-right font-medium">Weight</p>
					</th>
				{/if}
			</tr>
		</thead>
		<tbody class="block h-full max-h-full py-4">
			{#each factors as factor, i}
				<tr
					class="flex items-center hover:bg-whiten dark:hover:bg-boxdark-2 border-b border-stroke dark:border-strokedark"
				>
					<td class="w-[40%] py-4 pl-4 pr-4 lg:pl-10 xl:w-1/4"> {factor.name} </td>
					<td class="w-3/5 p-4">
						<Slider id="factor-{i}-rating" bind:value={factor.rating} min={1} max={5} />
					</td>
					{#if weightCustomizer}
						<td class="w-[35%] py-4 pl-4 pr-4 lg:pr-10 xl:w-[20%]">
							<Input
								class="basis-1/4"
								type="number"
								id="factor-{i}-weight"
								bind:value={factor.weight}
								min="0"
								max="3"
							/>
						</td>
					{/if}
				</tr>
			{/each}
		</tbody>
	</table>

	<div class="flex justify-between gap-4 py-4 pl-4 pr-4 lg:pl-10 lg:pr-10">
		<div>
			<div class="font-medium mb-2">Score</div>
			<div class="font-bold text-4xl">{normalizedScore}</div>
		</div>
		<div>
			<div class="font-medium mb-2">Grade</div>
			<div class="font-bold text-4xl">{grade}</div>
		</div>
	</div>
</div>
