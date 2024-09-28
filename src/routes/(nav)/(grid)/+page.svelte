<script>
	import { onMount } from 'svelte';
	import { Card } from '$lib/components/ui/card';
	import supabase from '$lib/db/db';
	import { BackgroundGradient } from '$lib/components/ui';

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

	// @ts-ignore
	const handleCardClick = (recipient) => {
		current_recipient = recipient;
		// Filter data to get the current recipient's details
		// @ts-ignore
		current_recipient_details = data.filter((item) => item.recipient === recipient).reverse();
		console.log('Current recipient:', current_recipient);
		console.log('Current recipient details:', current_recipient_details);
	};
</script>

<!-- Remove or comment out this line -->
<!-- {JSON.stringify(data)}
{JSON.stringify(current_recipient)} -->

{#if current_recipient}
	<h2 class="mb-4 text-2xl font-bold">Current Recipient Details</h2>
	<div class="space-y-4">
		{#each current_recipient_details as detail}
			<Card class="w-96 p-4">
				<div on:click={() => (detail.expanded = !detail.expanded)} class="cursor-pointer">
					<p><strong>Response:</strong> {detail.response}</p>
					<p><strong>Timestamp:</strong> {detail.timestamp}</p>
					{#if detail.expanded}
						<p><strong>ID:</strong> {detail.id}</p>
						<p><strong>Created At:</strong> {detail.created_at}</p>
						<p><strong>Recipient:</strong> {detail.recipient}</p>
						<p><strong>Last Chat:</strong> {detail.last_chat}</p>
						<p><strong>Route:</strong> {detail.route}</p>
					{/if}
				</div>
			</Card>
		{/each}
	</div>
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
