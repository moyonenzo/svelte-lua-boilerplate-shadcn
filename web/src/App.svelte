<script lang="ts">
    import VisibilityProvider from "./lib/components/layout/VisibilityProvider.svelte";
    import { debugData } from "$lib/utils/debugData";
    import { visibility } from "$lib/utils/stores";

    import * as Card from "$lib/components/ui/card/index.js";
    import { Button } from "$lib/components/ui/button";
    import { Separator } from "$lib/components/ui/separator";

    import { toast } from "svelte-sonner";
    import { Toaster } from "$lib/components/ui/sonner";
    import { ModeWatcher } from "mode-watcher";
    import { fetchNui } from "$lib/utils/fetchNui";

    // Needed to set as ui visible in browser for debug
    debugData([{ action: "setVisible", data: true }]);

    let clientCoords: { x: number; y: number; z: number };
    async function getClientData() {
        try {
            clientCoords = await fetchNui<{ x: number; y: number; z: number }>(
                "getClientData",
            );
        } catch (e) {
            clientCoords = { x: 0.0, y: 0.0, z: 0.0 };
            toast.error("An error occured while fetching player coords.");
        }
    }

    async function copyToClipboard() {
        try {
            await navigator.clipboard.writeText(
                `vector3(${clientCoords.x}, ${clientCoords.y}, ${clientCoords.z})`,
            );
            toast.success(
                "Player coordinates successfully copied to clipboard.",
            );
        } catch (e) {
            toast.error(
                "An error occured while copying player coords to clipboard.",
            );
        }
    }

    function close() {
        visibility.set(false);
        fetchNui("hideUI");
    }
</script>

<Toaster richColors />
<ModeWatcher />

<VisibilityProvider>
    <Card.Root class="w-full max-w-sm">
        <Card.Header>
            <Card.Title class="text-2xl">Title</Card.Title>
            <Card.Description>
                Some description below this title
            </Card.Description>
        </Card.Header>
        <Card.Content class="grid gap-4">
            {#await getClientData() then _}
                <p>
                    Player coordinates : ({clientCoords.x}, {clientCoords.y}, {clientCoords.z})
                </p>

                <Button
                    class="w-full"
                    variant="secondary"
                    on:click={getClientData}
                >
                    Update coordinates
                </Button>
                <Button
                    class="w-full"
                    variant="outline"
                    on:click={copyToClipboard}
                >
                    Copy coordinates to clipboard
                </Button>
                <Separator />
            {/await}
        </Card.Content>
        <Card.Footer>
            <Button class="w-full" on:click={close}>Close UI</Button>
        </Card.Footer>
    </Card.Root>
</VisibilityProvider>
