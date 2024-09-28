<script lang="ts">
	import { cn } from '$lib/utils';
	import { Motion } from 'svelte-motion';

	export let className: string | undefined = undefined;
	export let containerClassName: string | undefined = undefined;
	export let animate: boolean | undefined = true;

	const variants = {
		initial: {
			backgroundPosition: '0 50%'
		},
		animate: {
			backgroundPosition: ['0, 50%', '100% 50%', '0 50%']
		}
	};
</script>

<div class={cn('group relative p-[4px]', containerClassName)}>
	<Motion
		let:motion
		variants={animate ? variants : undefined}
		initial={animate ? 'initial' : undefined}
		animate={animate ? 'animate' : undefined}
		transition={animate
			? {
					duration: 5,
					repeat: Infinity,
					repeatType: 'reverse'
				}
			: undefined}
		style={{
			backgroundSize: animate ? '400% 400%' : undefined
		}}
	>
		<div
			use:motion
			class={cn(
				'absolute inset-0 z-[1] rounded-3xl opacity-50 blur-2xl transition duration-500 group-hover:opacity-70',
				'bg-[radial-gradient(circle_farthest-side_at_0_100%,#00ccb177,transparent),radial-gradient(circle_farthest-side_at_100%_0,#7b61ff77,transparent),radial-gradient(circle_farthest-side_at_100%_100%,#ffc41477,transparent),radial-gradient(circle_farthest-side_at_0_0,#1ca0fb77,#14131677)]'
			)}
		/>
	</Motion>

	<Motion
		let:motion
		variants={animate ? variants : undefined}
		initial={animate ? 'initial' : undefined}
		animate={animate ? 'animate' : undefined}
		transition={animate
			? {
					duration: 5,
					repeat: Infinity,
					repeatType: 'reverse'
				}
			: undefined}
		style={{
			backgroundSize: animate ? '400% 400%' : undefined
		}}
	>
		<div
			use:motion
			class={cn(
				'absolute inset-0 z-[1] rounded-3xl',
				'bg-[radial-gradient(circle_farthest-side_at_0_100%,#00ccb177,transparent),radial-gradient(circle_farthest-side_at_100%_0,#7b61ff77,transparent),radial-gradient(circle_farthest-side_at_100%_100%,#ffc41477,transparent),radial-gradient(circle_farthest-side_at_0_0,#1ca0fb77,#14131677)]'
			)}
		/>
	</Motion>

	<div class={cn('relative z-10', className)}><slot /></div>
</div>
