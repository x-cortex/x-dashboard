<script lang="ts">
	import { page } from '$app/stores';
	import { siteConfig } from '$lib/config/site';
	import { cn } from '$lib/utils';
	import { Icons } from '$lib/icons';
</script>

<div class="mr-4 hidden md:flex">
	<a href="/" class="mr-10 flex items-center">
		<Icons.logoDark class="mr-4 hidden rounded-full dark:block" />
		<Icons.logo class="mr-4 rounded-full dark:hidden" />
		<span class="hidden text-[15px] font-medium sm:inline-block lg:text-base">
			{siteConfig.name}
		</span>
	</a>
	<nav class="flex items-center space-x-6 text-sm font-medium">
		<a
			href="/x-whatsapp"
			class={cn(
				'font-light transition-colors hover:text-foreground/80',
				$page.url.pathname === '/x-whatsapp' ? 'text-foreground' : 'text-foreground/60'
			)}
		>
			x-whatsapp
		</a>
		<a
			href="/x-telegram"
			class={cn(
				'font-light transition-colors hover:text-foreground/80',
				$page.url.pathname === '/x-whatsapp' ? 'text-foreground' : 'text-foreground/60'
			)}
		>
			x-telegram
		</a>
		<a
			href="https://x-cortex-docs.vercel.app"
			class={cn(
				'font-light transition-colors hover:text-foreground/80',
				$page.url.pathname.startsWith('/docs') ? 'text-foreground' : 'text-foreground/60'
			)}
		>
			x-cortex-docs
		</a>

		<!-- <a
			href={siteConfig.links.github}
			target="_blank"
			rel="noopener noreferrer"
			class={cn('hidden text-foreground/60 transition-colors hover:text-foreground/80 xl:block')}
		>
			GitHub
		</a> -->
	</nav>
</div>
