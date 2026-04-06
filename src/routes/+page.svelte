<script lang="ts">
	import * as Plot from "@observablehq/plot";
	import data from "../data/press-freedom-vs-animal-welfare-grade.ts";

	let container: HTMLDivElement;

	$effect(() => {
		const chart = Plot.plot({
			width: 800,
			height: 500,
			marginTop: 40,
			marginBottom: 40,
			fx: {
				label: null,
				domain: ["B", "C", "D", "E"],
			},
			y: {
				label: "Press freedom score",
				grid: true,
			},
			marks: [
				Plot.image(
					data,
					Plot.dodgeX({
						y: "press_freedom_score",
						fx: "grade",
						r: 15,
						src: "flag_url",
						preserveAspectRatio: "xMidYMid slice",
						title: (d: typeof data[number]) =>
						`${d.country}\nAnimal Protection Index Grade: ${d.grade}\nRSF Press Freedom Index: ${d.press_freedom_score}`,
					})
				),
			],
		});
		container.replaceChildren(chart);
		return () => chart.remove();
	});
</script>

<h1>Do countries with more press freedom treat their animals better?</h1>
<p>With a <a href="https://en.wikipedia.org/wiki/Spearman's_rank_correlation_coefficient">Spearman's rank correlation coefficient</a> of 0.77, a country's press freedom score and its animal welfare grade are strongly correlated — countries that protect press freedom also tend to have stronger animal welfare protections, though there are notable exceptions like Mexico and Turkey.</p>
<div bind:this={container} role="img" aria-label="Chart of press freedom scores by animal welfare grade, with country flags as marks"></div>
<div class="x-axis-arrows">
	<span class="arrow-left">&larr; Better animal welfare</span>
	<span class="arrow-right">Worse animal welfare &rarr;</span>
</div>
<div class="sources" style="margin-top: 40px;">
	<div>🔗 <a href="https://api.worldanimalprotection.org/">World Animal Protection Animal Protection Index</a></div>
	<div>🔗 <a href="https://rsf.org/en/index">RSF World Press Freedom Index</a></div>
</div>
<footer style="margin-top: 40px;">Created by <a href="https://www.nicoledesignsdata.net">Nicole Mark</a> for #30DayChartChallenge 2026 Day 6 Reporters Without Borders Data Day</footer>

<style>
	.x-axis-arrows {
		display: flex;
		justify-content: space-between;
		max-width: 800px;
		margin-top: -1.5em;
		padding: 0 3em;
		font-size: 10px;
		color: #666;
	}
</style>
