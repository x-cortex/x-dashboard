<script>
	import { BackgroundGradient, BackgroundGradientLess } from '$lib/components/ui';
	import { Card } from '$lib/components/ui/card';
	import supabase from '$lib/db/db';
	import { onMount } from 'svelte';

	// @ts-ignore
	let data = []; // Initialize data as an empty array
	// @ts-ignore
	let allRecipients = []; // Array for unique recipients
	// @ts-ignore
	let current_recipient = null;
	// @ts-ignore
	let current_recipient_details = null;

	onMount(async () => {
		await fetchData();
	});

	const fetchData = async () => {
		const { data: supa, error } = await supabase.from('chat_data').select('*');

		if (error) {
			console.error('Error fetching data:', error);
		} else {
			console.log('Supa', supa);
			data = supa; // Assuming supa is an array of objects
			updateAllRecipients();
		}
	};

	const updateAllRecipients = () => {
		// Create a Set of unique recipients, then convert it back to an array
		// @ts-ignore
		allRecipients = [...new Set(data.map((item) => item.recipient))];
		console.log('All unique recipients:', allRecipients);
	};

	let selectedDetail = null;

	// @ts-ignore
	const handleCardClick = (recipient) => {
		current_recipient = recipient;
		// Filter data to get the current recipient's details
		// @ts-ignore
		current_recipient_details = data.filter((item) => item.recipient === recipient).reverse();
		selectedDetail = null; // Reset selected detail when changing recipient
		console.log('Current recipient:', current_recipient);
		console.log('Current recipient details:', current_recipient_details);
	};

	// @ts-ignore
	const handleDetailClick = (detail) => {
		selectedDetail = detail;
	};

	const handleBackClick = () => {
		current_recipient = null;
		current_recipient_details = null;
		selectedDetail = null;
	};
</script>

<!-- Remove or comment out this line -->
<!-- {JSON.stringify(data)}
{JSON.stringify(current_recipient)} -->

{#if current_recipient}
	<section class="flex w-full justify-around">
		<button
			on:click={handleBackClick}
			class="dark:hover:bg-gray-800Response absolute left-4 top-20 flex items-center rounded-md border border-gray-200 bg-white px-4 py-2 text-gray-800 transition-colors hover:bg-gray-300 dark:border-gray-600 dark:bg-zinc-900 dark:text-gray-200"
		>
			<svg
				xmlns="http://www.w3.org/2000/svg"
				class="mr-2 h-5 w-5"
				fill="none"
				viewBox="0 0 24 24"
				stroke="currentColor"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M15 12H3m0 0l6-6m-6 6l6 6"
				/>
			</svg>
		</button>
		<div
			class="ml-20 flex max-h-[800px] flex-col justify-center space-y-4 overflow-y-auto p-10 pt-96"
		>
			{#each current_recipient_details as detail}
				<!-- Skip the first item -->
				<BackgroundGradientLess
					className="rounded-[22px] p-6 bg-white dark:bg-zinc-900 max-w-sm mx-auto w-full"
				>
					<div
						on:click={() => handleDetailClick(detail)}
						class="flex h-full cursor-pointer flex-col justify-between"
					>
						<p class="flex-1"><strong>Response:</strong> {detail.response}</p>
						<p class="flex-1"><strong>Timestamp:</strong> {detail.timestamp}</p>
					</div>
				</BackgroundGradientLess>
			{/each}
		</div>
		<div class="flex max-h-[800px] flex-col items-center justify-start pt-8">
			<BackgroundGradient
				className="flex h-full min-h-[700px] w-full min-w-[800px] max-w-[800px] flex-col p-8 pt-10 rounded-[22px] bg-white dark:bg-zinc-900 mx-auto "
			>
				{#if selectedDetail}
					<h3 class="mb-4 text-3xl font-semibold text-gray-900 dark:text-white">Selected Detail</h3>
					{#each Object.entries(selectedDetail) as [key, value]}
						<div
							class="mb-4 rounded-lg border border-gray-200 bg-white p-2 shadow-sm dark:border-gray-700 dark:bg-gray-800"
						>
							<p class="overflow-hidden text-ellipsis whitespace-nowrap">
								<strong class="text-wrap text-lg font-medium text-gray-800 dark:text-gray-200"
									>{key}:</strong
								>
								<span class="text-wrap text-base text-gray-600 dark:text-gray-400">{value}</span>
							</p>
						</div>
					{/each}
				{:else}
					<p class="mt-4 text-center text-lg text-gray-500 dark:text-gray-400">
						Select a Card to view more information
					</p>
				{/if}
			</BackgroundGradient>
		</div>
	</section>
{:else}
	<div class="container mx-auto max-w-7xl space-y-12 px-4 py-8 sm:px-6 lg:px-8">
		<section>
			<h2 class="mb-6 text-3xl font-bold">Whatsapp</h2>
			<div class="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3">
				{#each allRecipients as recipient, i}
					<BackgroundGradient
						className="rounded-[22px] p-6 bg-white dark:bg-zinc-900 max-w-sm mx-auto w-full"
					>
						<button class="h-full w-full text-left" on:click={() => handleCardClick(recipient)}>
							<p
								class="mb-3 text-center text-xl font-semibold text-black dark:text-neutral-200 sm:text-2xl"
							>
								{recipient || `Whatsapp ${i + 1}`}
							</p>
							<p class="text-center text-sm text-neutral-600 dark:text-neutral-400">
								Click to view chat history and details
							</p>
						</button>
					</BackgroundGradient>
				{/each}
			</div>
		</section>

		<section>
			<h2 class="mb-6 text-3xl font-bold">Telegram</h2>
			<p class="text-sm text-neutral-600 dark:text-neutral-400">
				Telegram integration is coming soon. Stay tuned for updates!
			</p>
		</section>
	</div>
{/if}
