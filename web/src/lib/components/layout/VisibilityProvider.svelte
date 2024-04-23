<script lang="ts">
    import { useNuiEvent } from "$lib/utils/useNuiEvent";
    import { fetchNui } from "$lib/utils/fetchNui";
    import { onMount } from "svelte";
    import { visibility } from "$lib/utils/stores";

    let isVisible: boolean;

    visibility.subscribe((visible) => {
        isVisible = visible;
    });

    useNuiEvent<boolean>("setVisible", (visible) => {
        visibility.set(visible);
    });

    onMount(() => {
        const keyHandler = (e: KeyboardEvent) => {
            if (isVisible && ["Escape"].includes(e.code)) {
                fetchNui("hideUI");
                visibility.set(false);
            }
        };

        window.addEventListener("keydown", keyHandler);

        return () => window.removeEventListener("keydown", keyHandler);
    });
</script>

<main class="flex flex-col h-screen items-center justify-center">
    {#if isVisible}
        <slot />
    {/if}
</main>
