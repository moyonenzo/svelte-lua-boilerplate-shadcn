<script lang="ts">
    import VisibilityProvider from "./lib/components/layout/VisibilityProvider.svelte";
    import { debugData } from "$lib/utils/debugData";
    import { visibility } from "$lib/utils/stores";

    import * as Card from "$lib/components/ui/card/index.js";
    import { Button } from "$lib/components/ui/button";

    import { ModeWatcher } from "mode-watcher";
    import { fetchNui } from "$lib/utils/fetchNui";

    // Needed to set as ui visible in browser for debug
    debugData([{ action: "setVisible", data: true }]);

    function close() {
        visibility.set(false);
        fetchNui("hideUI");
    }
</script>

<ModeWatcher />

<VisibilityProvider>
    <Card.Root class="w-full max-w-sm">
        <Card.Header>
            <Card.Title class="text-2xl">Title</Card.Title>
            <Card.Description>
                Some description below this title
            </Card.Description>
        </Card.Header>
        <Card.Content class="grid gap-4"></Card.Content>
        <Card.Footer>
            <Button class="w-full" on:click={close}>Close UI</Button>
        </Card.Footer>
    </Card.Root>
</VisibilityProvider>
