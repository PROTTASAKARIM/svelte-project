<script>
	import { onMount } from 'svelte';

	// Define a reactive variable to track the screen width
	let screenWidth = 0;

	// Add an event listener to update the screen width when it changes
	onMount(() => {
		screenWidth = window.innerWidth;

		window.addEventListener('resize', () => {
			screenWidth = window.innerWidth;
		});
	});
</script>

<div class="container">
	<!-- Content that adapts to different screen sizes -->
	{#if screenWidth >= 640}
		<div class="bg-gray-100 min-h-screen">
			<div class="grid grid-cols-12 grid-flow-col w-screen">
				<div class="col-span-6 text-center shadow-md">
					<div class="grid grid-cols-6 h-full">
						<div class="col-span-2">
							<div class="grid grid-rows-5 h-full">
								<div class="row-span-3 bg-blue-400 h-full">3*2</div>
								<div class="row-span-2 bg-teal-400 h-full">2*2</div>
							</div>
						</div>
						<div class="col-span-4 bg-rose-100 h-full">
							<p class="h-full">5*4</p>
						</div>
						<div class="col-span-6 bg-fuchsia-400 row-span-1">
							<p class="text-white">1*6</p>
						</div>
					</div>
				</div>

				<div class="col-span-6 text-center shadow-md h-screen">
					<div class="col-span-6 text-center shadow-md h-screen">
						<div class="grid grid-rows-2 grid-cols-6 h-full">
							<!-- 6*2 Grid -->
							<div class="col-span-6 row-span-1 bg-blue-400">
								<p class="text-white">6*2</p>
							</div>
							<!-- <div class="col-span-4 row-span-2 bg-yellow-100 flex justify-center items-center">
								<div class="w-72 h-72 bg-yellow-100 text-white">
									<div class="w-72 h-72 bg-blue-900 flex items-center justify-center">
										<div
											class="w-40 h-40 bg-yellow-100 rounded-full flex items-center justify-center"
										>
											<p>4*4</p>
										</div>
									</div>
								</div>
							</div> -->
							<div class="col-span-4 row-span-2 bg-yellow-100 flex justify-center items-center">
								<div class="w-72 h-72 bg-yellow-100 text-white">
									<div class="w-72 h-72 bg-blue-900 flex items-center justify-center">
										<div
											class="w-40 h-40 bg-yellow-100 rounded-full flex items-center justify-center"
										>
											<div class="mountain">
												<div class="peak-small" />
												<div class="peak-large" />
												<div class="mountain-base" />
											</div>
										</div>
									</div>
								</div>
							</div>

							<!-- 2*4 Grid -->
							<div class="col-span-2 row-span-2 bg-emerald-400">
								<p class="text-white">2*4</p>
							</div>
						</div>
					</div>
				</div>

				<!-- Add more grid items here if needed -->
			</div>
		</div>
	{:else}
		<div class="bg-gray-100 min-h-screen">
			<div class="grid grid-cols-1 grid-flow-col w-screen" />
			<div class="col-span-4 w-full h-72 bg-rose-100">
				<p class="h-full">5*4</p>
			</div>
			<div class="col-span-4 row-span-2 bg-yellow-100 flex justify-center items-center">
				<div class="w-72 h-72 bg-yellow-100 text-white">
					<div class="w-72 h-72 bg-blue-900 flex items-center justify-center">
						<div class="w-40 h-40 bg-yellow-100 rounded-full flex items-center justify-center">
							<div class="mountain">
								<div class="peak-small" />
								<div class="peak-large" />
								<div class="mountain-base" />
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	{/if}
</div>

<!-- <p class="text-gray-600">This is a responsive Svelte app with Tailwind CSS.</p> -->

<style>
	/* Define custom styles based on screen width using media queries */
	@media (min-width: 640px) {
		/* Styles for desktop view */
		.container {
			height: 100vh;
		}
		.box {
			width: 300px;
			height: 300px;
			background-color: #ffc107; /* Change this to your desired background color */
			display: flex;
			align-items: center;
			justify-content: center;
			text-align: center;
			border: 2px solid #333; /* Optional: Add a border */
			box-sizing: border-box;
		}
		.mountain {
			position: relative;
			width: 100%;
			height: 60px;
			bottom: -50px;
			border-left: 20px solid transparent;
			border-right: 20px solid transparent;
			border-bottom: 20px solid #fe5f55; /* Color fill for the mountain */
		}
		.peak-small,
		.peak-large {
			position: absolute;
			top: -10px;
			width: 0;
			height: 0;
			border-left: 15px solid transparent;
			border-right: 15px solid transparent;
		}
		.peak-small {
			left: 100%;
			border-bottom: 10px solid #fe5f55; /* Color fill for the small peak */
		}

		.peak-large {
			left: 60%;
			border-bottom: 20px solid #fe5f55; /* Color fill for the large peak */
			border-top: 0; /* Remove the top border to create a triangle */
			height: 0; /* Reset height to 0 to create a triangle */
			width: 0; /* Reset width to 0 to create a triangle */
			border-left: 20px solid transparent; /* Adjust the size of the triangle */
			border-right: 20px solid transparent; /* Adjust the size of the triangle */
			border-bottom: 30px solid #fe5f55; /* Color fill for the triangle */
		}
	}

	@media (max-width: 639px) {
		/* Styles for mobile view */
		.container {
			/* padding: 1rem; */
		}
	}
</style>
