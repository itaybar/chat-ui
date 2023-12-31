<script lang="ts">
	import { base } from "$app/paths";
	import { page } from "$app/stores";
	import { PUBLIC_APP_DESCRIPTION, PUBLIC_APP_NAME } from "$env/static/public";
	import Modal from "$lib/components/Modal.svelte";
	import { onMount } from "svelte";
	import Logo from "./icons/Logo.svelte";

	let form: HTMLFormElement;
	onMount(() => {
		form.submit();
	});
</script>

<Modal on:close>
	<div
		class="flex w-full flex-col items-center gap-6 bg-gradient-to-b from-primary-500/40 via-primary-500/10 to-primary-500/0 px-5 pb-8 pt-9 text-center"
	>
		<h2 class="flex items-center text-2xl font-semibold text-gray-800">
			<Logo classNames="mr-1" />
			{PUBLIC_APP_NAME}
		</h2>
		<p class="text-lg font-semibold leading-snug text-gray-800" style="text-wrap: balance;">
			{PUBLIC_APP_DESCRIPTION}
		</p>
		<form
			action="{base}/{$page.data.loginRequired ? 'login' : 'settings'}"
			target="_parent"
			method="POST"
			class="flex w-full flex-col items-center gap-2"
			bind:this={form}
		/>
	</div>
</Modal>
