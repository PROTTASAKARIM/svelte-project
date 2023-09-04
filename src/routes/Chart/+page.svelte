<script>
	import { onMount } from 'svelte';
	import axios from 'axios';
	import Chart from 'chart.js/auto'; // Import Chart.js
	import { countryDataStore } from '../../lib/countryDataStore';

	onMount(async () => {
		try {
			const response = await axios.get('https://restcountries.com/v3.1/all');
			const countryData = response.data;
			countryDataStore.set(countryData);

			console.log('countryData', countryData);
			createPolarAreaChart(countryData);
		} catch (error) {
			console.error('An error occurred while fetching data:', error);
		}
	});

	function createPolarAreaChart(data) {
		const sortedData = data.sort((a, b) => b.population - a.population);
		const top10Data = sortedData.slice(0, 10);

		const ctx = document.getElementById('polarAreaChart').getContext('2d');
		new Chart(ctx, {
			type: 'polarArea',
			data: {
				labels: top10Data.map((country) => country.name.common),
				datasets: [
					{
						label: 'Population',
						data: top10Data.map((country) => country.population),
						backgroundColor: [
							'rgba(0, 0, 0, 0.7)', // Black
							'rgba(105, 105, 105, 0.7)', // Dim Gray
							'rgba(169, 169, 169, 0.7)', // Dark Gray
							'rgba(192, 192, 192, 0.7)', // Silver
							'rgba(105, 105, 105, 0.7)', // Dim Gray
							'rgba(0, 0, 0, 0.7)', // Black
							'rgba(169, 169, 169, 0.7)', // Dark Gray
							'rgba(192, 192, 192, 0.7)', // Silver
							'rgba(0, 0, 0, 0.7)', // Black
							'rgba(105, 105, 105, 0.7)' // Dim Gray
						]
					}
				]
			},
			options: {
				responsive: true,
				legend: {
					display: true,
					position: 'bottom', // Place legend at the bottom
					labels: {
						boxWidth: 10, // Adjust the box width if needed
						fontSize: 10 // Adjust the font size if needed
					}
				}
			}
		});
	}
</script>

<div class=" w-auto flex" style="font-size: 1rem;">
	<div class="w-2/3 overflow-x-auto">
		<div class="bg-white shadow-md rounded-lg overflow-hidden">
			<table class="w-full border-collapse border border-gray-300">
				<thead>
					<tr>
						<th class="p-2 bg-gray-200 border border-gray-300">Flag</th>
						<th class="p-2 bg-gray-200 border border-gray-300">Name</th>
						<th class="p-2 bg-gray-200 border border-gray-300">Population</th>
						<th class="p-2 bg-gray-200 border border-gray-300">CIOC</th>
						<th class="p-2 bg-gray-200 border border-gray-300">UN Member Status</th>
						<th class="p-2 bg-gray-200 border border-gray-300">Currencies</th>
						<th class="p-2 bg-gray-200 border border-gray-300">Languages</th>
					</tr>
				</thead>
				<tbody>
					{#each $countryDataStore as country (country.name.common)}
						<tr>
							<td class="p-2 border border-gray-300">
								<img src={country.flags.png} alt="Flag" width="30" height="20" />
							</td>
							<td class="p-2 border border-gray-300">{country.name.common}</td>
							<td class="p-2 border border-gray-300">{country.population}</td>
							<td class="p-2 border border-gray-300">
								{#if country.cioc}
									{country.cioc}
								{:else}
									N/A
								{/if}
							</td>

							<td class="p-2 border border-gray-300">{country.unMember ? 'Yes' : 'No'}</td>
							<td class="p-2 border border-gray-300">
								{#if country.currencies}
									{Object.keys(country.currencies).join(', ')}
								{:else}
									N/A
								{/if}
							</td>

							<td class="p-2 border border-gray-300">
								{#if country.languages}
									{Object.keys(country.languages).join(', ')}
								{:else}
									N/A
								{/if}
							</td>
						</tr>
					{/each}
				</tbody>
			</table>
		</div>
	</div>
	<div class="w-1/3 p-4">
		<div class="bg-white shadow-md rounded-lg overflow-hidden">
			<div class="p-4">
				<h2 class="text-xl font-semibold mb-4 border-b border-gray-300">Countries</h2>
				<canvas id="polarAreaChart" />
			</div>
		</div>
	</div>
</div>
