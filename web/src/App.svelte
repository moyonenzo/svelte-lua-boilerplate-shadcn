<script lang="ts">
    import VisibilityProvider from "./lib/components/layout/VisibilityProvider.svelte";
    import { debugData } from "$lib/utils/debugData";

    import * as Card from "$lib/components/ui/card/index.js";
    import { Button } from "$lib/components/ui/button";

    import { ModeWatcher } from "mode-watcher";

    debugData([
        {
            action: "setVisible",
            data: true,
        },
    ]);
</script>

<ModeWatcher />

<VisibilityProvider>
    <Card.Root class="w-full max-w-sm">
        <Card.Header>
            <Card.Title class="text-2xl">Title</Card.Title>
            <Card.Description
                >Some description below this title</Card.Description
            >
        </Card.Header>
        <Card.Content class="grid gap-4"></Card.Content>
        <Card.Footer>
            <Button class="w-full">Click me button</Button>
        </Card.Footer>
    </Card.Root>
</VisibilityProvider>
