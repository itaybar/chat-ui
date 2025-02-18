<script lang="ts">
	import { PUBLIC_APP_NAME } from "$env/static/public";
	import { PUBLIC_ANNOUNCEMENT_BANNERS } from "$env/static/public";
	import { PUBLIC_APP_DESCRIPTION } from "$env/static/public";
	import Logo from "$lib/components/icons/Logo.svelte";
	import { createEventDispatcher } from "svelte";
	import IconGear from "~icons/bi/gear-fill";
	import AnnouncementBanner from "../AnnouncementBanner.svelte";
	import type { Model } from "$lib/types/Model";
	import { findCurrentModel } from "$lib/utils/models";
	import { base } from "$app/paths";
	import { useSettingsStore } from "$lib/stores/settings";
	import JSON5 from "json5";

	export let currentModel: Model;
	export let models: Model[];

	const settings = useSettingsStore();

	$: currentModelMetadata = findCurrentModel(models, $settings.activeModel);

	const announcementBanners = PUBLIC_ANNOUNCEMENT_BANNERS
		? JSON5.parse(PUBLIC_ANNOUNCEMENT_BANNERS)
		: [];

	const dispatch = createEventDispatcher<{ message: string }>();
</script>

<div class="my-auto grid gap-8 lg:grid-cols-3">
	<div class="lg:col-span-1">
		<div>
			<div class="mb-3 flex items-center text-2xl font-semibold">
				<Logo classNames="mr-1 flex-none" />
				{PUBLIC_APP_NAME}
				<!-- <div
					class="ml-3 flex h-6 items-center rounded-lg border border-gray-100 bg-gray-50 px-2 text-base text-gray-400 dark:border-gray-700/60 dark:bg-gray-800"
				>
					v{PUBLIC_VERSION}
				</div> -->
			</div>
			<p class="text-base text-gray-600 dark:text-gray-400">
				{PUBLIC_APP_DESCRIPTION ||
					"Making the community's best AI chat models available to everyone."}
			</p>
		</div>
	</div>
	<div class="lg:col-span-2 lg:pl-24">
		{#each announcementBanners as banner}
			<AnnouncementBanner classNames="mb-4" title={banner.title}>
				<!-- <a
					target="_blank"
					href={banner.linkHref}
					class="mr-2 flex items-center underline hover:no-underline"
					><CarbonArrowUpRight class="mr-1.5 text-xs" /> {banner.linkTitle}</a
				> -->
			</AnnouncementBanner>
		{/each}
		<a href="{base}/settings/{currentModel.id}">
			<div
				class="overflow-hidden rounded-xl border hover:bg-gray-100 dark:border-gray-600 dark:border-gray-800 dark:bg-gray-800 dark:hover:bg-gray-600"
			>
				<div class="flex p-3">
					<div>
						<div class="text-sm text-gray-600 dark:text-gray-400">Current Model</div>
						<div class="font-semibold">{currentModel.displayName}</div>
					</div>
					<div class="btn ml-auto flex h-7 w-7 self-start rounded-full"><IconGear /></div>
				</div>
				<!-- <ModelCardMetadata variant="dark" model={currentModel} /> -->
			</div>
		</a>
	</div>
	{#if currentModelMetadata.promptExamples}
		<div class="lg:col-span-3 lg:mt-6">
			<p class="mb-3 text-gray-600 dark:text-gray-300">Examples</p>
			<div class="grid gap-3 lg:grid-cols-3 lg:gap-5">
				{#each currentModelMetadata.promptExamples as example}
					<button
						type="button"
						class="rounded-xl border bg-gray-50 p-2.5 text-gray-600 hover:bg-gray-100 sm:p-4 dark:border-gray-800 dark:bg-gray-800 dark:text-gray-300 dark:hover:bg-gray-700"
						on:click={() => dispatch("message", example.prompt)}
					>
						{example.title}
					</button>
				{/each}
			</div>
		</div>{/if}
</div>
