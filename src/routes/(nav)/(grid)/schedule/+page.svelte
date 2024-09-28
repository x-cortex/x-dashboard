<script>
	import { BackgroundGradient } from '$lib/components/ui';
	import supabase from '$lib/db/db';
	import { onMount } from 'svelte';

	let recipient = '';
	let chat = '';
	let sendTime = '';
	let schedules = [];

	onMount(async () => {
		await fetchSchedules();
	});

	const fetchSchedules = async () => {
		const { data, error } = await supabase.from('schedule').select('*');
		if (error) {
			console.error('Error fetching schedules:', error);
		} else {
			schedules = data;
		}
	};

	const handleSubmit = async () => {
		const { data, error } = await supabase
			.from('schedule')
			.insert([{ recipient, chat, send_time: sendTime }]);

		if (error) {
			console.error('Error inserting schedule:', error);
		} else {
			console.log('Schedule inserted successfully:', data);
			recipient = '';
			chat = '';
			sendTime = '';
			await fetchSchedules();
		}
	};
</script>

<div class="container mx-auto max-w-6xl">
	<div class="w-full max-w-xl space-y-12 px-4 py-8 sm:px-6 lg:px-8">
		<section>
			<h2 class="mb-6 text-3xl font-bold">Schedule a Message</h2>
			<BackgroundGradient className="rounded-[22px] p-6 bg-white dark:bg-zinc-900 max-w-lg mx-auto">
				<form on:submit|preventDefault={handleSubmit} class="w-4xl min-w-full space-y-4">
					<div>
						<label
							for="recipient"
							class="block text-sm font-medium text-gray-700 dark:text-gray-300">Recipient</label
						>
						<input
							type="text"
							id="recipient"
							bind:value={recipient}
							required
							class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 dark:border-gray-600 dark:bg-gray-700 dark:text-white"
						/>
					</div>
					<div>
						<label for="chat" class="block text-sm font-medium text-gray-700 dark:text-gray-300"
							>Message</label
						>
						<textarea
							id="chat"
							bind:value={chat}
							required
							class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 dark:border-gray-600 dark:bg-gray-700 dark:text-white"
						></textarea>
					</div>
					<div>
						<label for="sendTime" class="block text-sm font-medium text-gray-700 dark:text-gray-300"
							>Send Time</label
						>
						<input
							type="datetime-local"
							id="sendTime"
							bind:value={sendTime}
							required
							class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 dark:border-gray-600 dark:bg-gray-700 dark:text-white"
						/>
					</div>
					<button
						type="submit"
						class="w-full rounded-md bg-blue-600 px-4 py-2 text-white hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 dark:bg-blue-500 dark:hover:bg-blue-600"
					>
						Schedule Message
					</button>
				</form>
			</BackgroundGradient>
		</section>
	</div>

	<div class="container mx-auto max-w-6xl space-y-12 px-4 py-8 sm:px-6 lg:px-8">
		<section>
			<h2 class="mb-6 text-3xl font-bold">Scheduled Messages</h2>
			<div class="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3">
				{#each schedules as schedule}
					<BackgroundGradient
						className="rounded-[22px] p-6 bg-white dark:bg-zinc-900 max-w-sm mx-auto w-full"
					>
						<div class="space-y-2">
							<p class="text-lg font-semibold text-black dark:text-neutral-200">
								To: {schedule.recipient}
							</p>
							<p class="text-sm text-neutral-600 dark:text-neutral-400">
								Message: {schedule.chat}
							</p>
							<p class="text-sm text-neutral-600 dark:text-neutral-400">
								Send Time: {new Date(schedule.send_time).toLocaleString()}
							</p>
						</div>
					</BackgroundGradient>
				{/each}
			</div>
		</section>
	</div>
</div>
